---
layout: post
title: How I set up this blog
date: '2022-07-24 05:43:18'
tags:
- project
---

### The blogging technologies I settled with 

I have done web development professionally, but, I want to get this blog up and spinning and jump straight to writing. So, despite still having nightmares about my friend Krish, asking me in highschool why I couldn't have just used wix for my 6 months long full stack project, this time, I will stand on the shoulders of blogging platforms (he has since written a 20 page apology letter, and if I've had a lapse of memory, he certainly should).

I chose to use Ghost as the base of my blog. For my non technical readers, think of Ghost as a large two-parted process running on some remote computer: One process displays your blog, the other allows you to manage your blog, so adding posts, sending newsletters, etc, all from an admin panel. For my technical readers, Ghost uses NGINX to serve your blog, provides a Medium.com like editor to create Markdown posts, and maintains a robust API for everything to be done programmatically. &nbsp;

So, my only blogging technology is Ghost.

### Self hosting Ghost 

If you don't self host Ghost, you are asking Ghost.org to host your blog and use Ghost.org's servers. The pros? A sense of security. Ghost.org is going to keep the Ghost software (your blog) running on their servers up to date. Ghost.org does the madness of managing SSL certificates, CDN networks, and mailing services for you. The cons? It's costlier than self-hosting ($25/mo for creator option, $9/mo for starters).

If you self host Ghost, it's a bit cheaper ($6/mo on DigitalOcean). Extra pros? A sense of freedom. You can scale up your servers as you need, ssh into the servers, and add any customization (WARNING! Customization may be euphemism for fragility).

So, I followed the instructions [here](https://ghost.org/docs/install/digitalocean/) to host my blog on DigitalOcean. It's a "one click" configuration, but I still need to manually do the following:

- Generate SSL certficates so that my blog uses HTTPS.
- Configuring subdomains to go to my main domain. For example, if user goes to `www.unfooling.com`, I want to route them to `unfooling.com`.

Note that at this point, after following the hyperlinked instructions, I have `unfooling.com` pointing at my DigitalOcean server's IP address.

The former bullet point is done by directly using `https` in my URL during the initial "one click" configuration. If I accidentally used the `http` version, I can just run `ghost stop` and `ghost setup` again and input the `https` URL. Ghost, under the hood, uses [Let's Encrypt](https://letsencrypt.org/) to generate SSL certificates and add to NGINX for me. If I don't want to setup everything, I could specify the step to `ghost setup`, like `ghost setup nginx ssl`.

The latter is accomplished by first pointing Ghost to the non main URL (my main URL is the non-www one, so, `unfooling.com`, you probably want to do the same, since www is just a subdomain)

`ghost config url https://www.unfooling.com`

Then, since this happens to be `https`, I setup a SSL certificate

`ghost setup nginx ssl`

Now, I nano the NGINX configuration file at `/etc/nginx/sites-enabled/www.unfooling.com.conf`, append a 301 redirect to the main URL using `return 301 https://unfooling.com$request_uri;`, and remove the server block. My configuration file now looks like:

    server {
        listen 80;
        listen [::]:80;
        
        server_name www.unfooling.com;
        root /var/www/ghost/system/nginx-root; # Used for acme.sh SSL verification (https://acme.sh)
    
        location ~ /.well-known {
            allow all;
        }
    
        client_max_body_size 50m;
    
        return 301 https://unfooling.com$request_uri;
    }

Since this happens to be `https`, I also made the edits to `/etc/nginx/sites-enabled/www.unfooling.com-ssl`.

I used this list in my own research:

- [https://forum.ghost.org/t/redirect-www-to-non-www/20494](https://forum.ghost.org/t/redirect-www-to-non-www/20494)
- [https://codecurated.com/blog/how-to-set-up-a-self-hosted-ghost-blogging-platform/](https://codecurated.com/blog/how-to-set-up-a-self-hosted-ghost-blogging-platform/)

### **Developing a custom Ghost blog theme**

The official [Journal](https://github.com/TryGhost/Journal) theme is almost perfect.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2022/07/image.png" class="kg-image" alt loading="lazy" width="1000" height="930" srcset=" __GHOST_URL__ /content/images/size/w600/2022/07/image.png 600w, __GHOST_URL__ /content/images/2022/07/image.png 1000w" sizes="(min-width: 720px) 720px"></figure>

But, I want to replace the subscribe buttons with discord invite buttons. So, I `git clone` the official Journal repo, delete all occurrences of "subscribe", and replace them with

    // default.hbs
    <a class="gh-author-discord" href="https://discord.gg/RXDrfY3Svu" target="_blank" rel="noopener">{{> "icons/discord"}}</a>
    
    // icons/discord
    <svg>...</svg>
    
    // override.css
    .gh-author-discord { 
    	width: 32px; 
        ...
    }

Now, there are limits. For example, it seems like my &nbsp;`css` overrides on the subscription portal are ignored. I don't care since I am not using it. But, that's a benefit to self hosting, you can go the extra mile to find out where Ghost ignores certain overrides and disable that line!

At last, I `gulp zip` and upload the zip files in the admin portal to deploy the changes.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2022/07/image-9.png" class="kg-image" alt loading="lazy" width="1097" height="383" srcset=" __GHOST_URL__ /content/images/size/w600/2022/07/image-9.png 600w, __GHOST_URL__ /content/images/size/w1000/2022/07/image-9.png 1000w, __GHOST_URL__ /content/images/2022/07/image-9.png 1097w" sizes="(min-width: 720px) 720px"></figure>
### **Custom theme CI/CD**

The last sentence of the previous paragraph might not have many words, but will certainly weigh heavy on my devops engineers. Instead of having to run a command, open up the admin portal, and drag a drop a folder, I automate theme uploading with Ghost's Javascript SDK:

    const api = new GhostAdminAPI({
      url: https://unfooling.com,
      key: <your_staff_key>,
      version: "v5.0",
    });
    const data = { file: "dist/<your_package_json_name>.zip" };
    api.themes.upload(data).then(({ name }) => api.themes.activate(name));

To push this one step further (pun intended), I uploaded my theme [repo](https://github.com/BlastWind/unfooling-blog-theme) to github and added this [script](https://github.com/BlastWind/unfooling-blog-theme/blob/main/scripts/upload-and-activate-theme.mjs) to a github [workflow](https://github.com/BlastWind/unfooling-blog-theme/blob/main/.github/workflows/build-and-deploy-theme.yml) that runs on push to main. The script is a little different on my repo since I am using environment variables that get its values from github workflow injected secrets.

