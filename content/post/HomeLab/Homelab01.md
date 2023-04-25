---
title: "Homelab As it is 1/20/2020"
author: "Justin Brant"
# cover: "/img/cover.jpg"
tags: ["HomeLab"]
date: 2019-09-06T14:19:13-04:00
draft: false
---

# The long neglected homelab
Roughly 3 years ago my life was very different. We lived in a metropolitan area with decent internet and a nice space for my homelab. Skip ahead 3 years we have moved to Convoy, Oh population 1,053~. We live on the outskirts of our village and where I can get 25/2 DSL Connection. Oh and I have a 2 year old toddler who takes up a majority of my time. 

With the pretense being out of the way. My lab went from a screaming c7000 with a fabric back-end of a 16 blade monster to a R310, and 2 R710's. During the move as I knew dedicated power was a ways off and my time was very limited. It's currently not cable managed, only has 1 R710 up and working. Which is running on spinning rust and very rudimentary  configured. Heck the lab doesn't even currently have a managed switch. The R310 which was my utility host is no longer with the land of the living. May god rest its soul. 

After some recent gift from a friend. It has given me to motivation to sink some time back into my lab and get it up to snuff again. To where I feel I can lab "about anything" I would want to lab up. This is going to be my thought process and goal list of the lab in a several part series to get it back up and running. 

# Goals
* Get a managed switch in place. ( I have a 24 port 3750-X That can be used)
* Get both hosts matching in specs. 1 R710 has 128 GB of RAM and other has 48 GB of RAM
* Sort the rack to make more sense and grab some shelves for non-rackable equipment
* Cable manage the rack
* Get a dedicated power circuit setup for the lab
  * Get power distribution system for Rack
* Get more cables for rackmount KVM? because why not?
 
 # Plausible Goals
 * Get some SSD's in either a SAN? or for hosts themselves? VSAN( I like to use DRS power managment so this may not be an option ) ?
 * Determine viability of newer hardware? 
 * Rack mount APC vs the desktop one I have
 * 10GB Back bone? (Microtik maybe?)

# Plans currently 1-20-2020
1. Get 3750-X powered up and configured
   1. Layout VLAN plan for switch
      1. VLAN for IOT
      2. VLAN for DMZ (Internet modem and firewall connection)
      3. VLAN for LAB
      4. VLAN for Generally Trusted Network
      5. VLAN for Guest
   2. Rear mount switch and cable manage
   3. Cable manage anything else that can be done while back there
      1. Re-lay out power cables and label (Order colored ones?)
      2. Move network cables to opposite side of rack
2. Get R710's match specs
   1. Brian Graff was so kind to share RAM with my homelab so this task is easier #ShoutOut!
   2. Will order the remainder off Ebay or source locally
3. Determine viability of new drives or SAN of some kind (Open to suggestions)
   1. FreeNAS virtualized on main R710 host? 
   2. Storinator? 
   3. Synology?
