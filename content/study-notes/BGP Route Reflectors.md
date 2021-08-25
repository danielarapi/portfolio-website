---
title: "BGP Route Reflectors"
date: 2021-08-21T03:18:45-04:00
draft: false
author: "Daniel Arapi"
language: "English"
tags: ["study-notes", "bgp", "route-reflectors", "ibgp"]
---

A BGP-speaking router, by default, will not advertise an iBGP route to an iBGP neighbor. One solution for this issue is to create a full mesh of neighborships within an Autonomous System (AS). However, that approach doesn’t scale well.

A more scalable solution is to use a BGP Route Reflector, or BGP confedirations. 

Full-Mesh forumla: N(N-1)/2

Route reflectors have the special BGP ability to readvertise routes learned from an internal peer to other internal peers. So rather than requiring all internal peers to be fully meshed with each other, route reflection requires only that the route reflector be fully meshed with all internal peers.



