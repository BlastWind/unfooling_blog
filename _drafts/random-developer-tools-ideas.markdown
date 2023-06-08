---
layout: post
title: App/Tools ideas
---

### Debugger and tester in one for better code reading 

When we test, we are trying to test the correctness of a particular execution path. If we concern about the correctness a lot, we are going to throw a lot of inputs into the execution path.

When we debug, we are trying to either understand an execution path better or try to pinpoint where an error exists...

Why not combine the two?

Use case #1: We want to understand a code base just starting out.

- We go read the test cases, because it has `describe ... it ...` declarative syntaxes. We then use our tool to generate the execution path this test case will take. We can boot up the graphviz-diagram to clearly see the path taken for this particular declarative test case.

### Convert everything, Open source with plugins

There are things like CloudConvert that supports A LOT of plugins. However, they are not open sourced.

I want to start my own Converter website whose converters gets dynamically added from its source repo. Contributors have to specify things like:

    from_type: "PDF"
    to_type: "TXT"
    docker_image: docker.registry.asdf
    (OR, link to local script_path)
    script_path: src/convert

When contributors upload their plugin, they must also upload sample from documents, which my Converter master repo runs using the docker\_image or the script, and verify its validity against the sample to documents.

If those work, it's automatically mergable.

So this project requires some great devops.

As for the frontend, everytime there's push to the master branch, rebuild the frontend. etc.

### Open all links and group em

Browser tabs need a redesign. Browser tabs should be hierarchial.

