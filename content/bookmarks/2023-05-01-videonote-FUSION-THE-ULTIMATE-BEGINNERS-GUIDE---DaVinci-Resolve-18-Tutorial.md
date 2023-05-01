---
title: 'FUSION: THE ULTIMATE BEGINNERS GUIDE - DaVinci Resolve 18 Tutorial'
date: 2023-05-01T12:59:53.1346880Z
author: Miguel Alho
draft: false
url: /bookmarks/FUSION-THE-ULTIMATE-BEGINNERS-GUIDE
imageFolderId: 1102544836679958528
article:
  author:
    name: ''
    twitter: ''
    linkedIn: ''
    website: ''
  links: []
video:
  source: youtube
  id: MDpR2xluwvI
  link: https://www.youtube.com/embed/MDpR2xluwvI
  author:
    name: Casey Faris
    twitter: ''
    linkedIn: ''
    website: ''
  context:
    name: ''
    link: ''
tags: []
summary: I starting to learn a bit about DaVinci Resolve for some video tutorial production. Fusion has a node/graph based editor for compositing which reminds me a lot about pipelines and unreal engine blueprints.Quite interesting, readable and powerful. Still, overwelming for a beginner to the tool, like me. This tutorial is a great intro into what the components are in Fusion and how it all comes together.
notes:
- id: 0
  type: Note
  time: 00:30
  image: ''
  title: On Compositing
  content: >-
    Essentially, it's layering media elements (video, images, text, forms, effects, ...) onto each other to produce the final video frame (and frame sequence).


    Interesting enough, Fusion's compositing feature works is similar to a build pipeline - a concept familiar to us in programming. Each node in the pipeline is a process that generates an artifact or transforms some input artifact to it's output, and you can chain and merge each process in the pipeline. 


    One node's output is the next one's input. and each node's process gets applied to each frame of the input clip.


    Each node's properties can be accessed in the inspector window/panel.
  comment: ''
- id: 1
  type: Quote
  time: 06:00
  image: ''
  title: ''
  content: Nodes are a flowchart of what you want the program to do.
  comment: ''
- id: 2
  type: Slide
  time: 08:00
  image: 1102544836679958528/1102548039135920128.png
  title: ''
  content: The toast analogy (analog mode) - each node as a step to get to to the desired end result (toast).
  comment: ''
- id: 3
  type: Slide
  time: 10:00
  image: 1102544836679958528/1102548604456796160.png
  title: ''
  content: >-
    Media in node: input node with some media element from the bin

    Blur node : effect node that applies blur to whatever is attached to its input node

    Media Out node: output and final node with final rendered element.
  comment: ''
- id: 4
  type: Note
  time: 12:30
  image: ''
  title: ''
  content: >-
    The merge node merges content. One piece of content is input as the Merge node's "background layer", while the other is the "foreground layer" and is applied over the background. 


    Inputs can be simple nodes or the result of complex processing.


    Merges can also receive Masks to define where the merge occurs.
  comment: ''
- id: 5
  type: Slide
  time: 16:20
  image: 1102544836679958528/1102550280450342912.png
  title: ''
  content: ''
  comment: ''
- id: 6
  type: Note
  time: 18:00
  image: ''
  title: ''
  content: >-
    Merge node: 


    Yellow : background input

    Green: foreground input

    Blue: mask input
  comment: ''
- id: 7
  type: Slide
  time: 21:00
  image: 1102544836679958528/1102551292917579776.png
  title: ''
  content: ''
  comment: ''
- id: 8
  type: Note
  time: 28:00
  image: ''
  title: Viewers
  content: >-
    There are two viewers. You can look at different parts of the composite, which is great for debugging the composition.


    Each node can be placed on any of the viewers. Pressing 1 or 2 on the keyboard selects the viewer for the node. Typically you'll have Media Out on 2, but you can have any on each.
  comment: >-
    Keys:


    1 - put to viewer 1

    2 - put to viewer 2
- id: 9
  type: Note
  time: 30:00
  image: ''
  title: Node types
  content: >
    Can essentially be grouped into following groups:


    * Generators : create or import media (video, image, forms, text...)

    * Effects: process and transforms input and generates an output.

    * Merge: Combines elements (one input on top of another), and can be chained. Previous merge is generally a background to the next merge


    Underlays: allow us to group nodes for better composition organization.
  comment: >-
    Keys:

    Shift + Spacebar: Tool select for quickly adding node by name.
- id: 10
  type: Note
  time: 46:00
  image: ''
  title: ''
  content: Edit flow timeline, is, in a manner, a sequence of merges (upper layer is a merge onto the lower layer).
  comment: ''
- id: 11
  type: Slide
  time: 00:00
  image: 1102544836679958528/1102554919916797952.png
  title: example 1
  content: >
    main background: footage of car traveling.
    
    
    to add the parking lot foreground: 
    

    * import parking lot footage,

    * color correct it

    * merge it with empty background, but apply a polygon mask. Empty background is used, so that merge has a background input.


    to add the shadow to the car in the parking,

    * polygon creates a mask on a background node to define the shadow form

    * merge the shadow with the previous comp, with adequate opacity and apply mode (Multiply)


    Planar transform tracks the motion, so the foreground layer comp moves along with the background camera travel.
  comment: ''
- id: 12
  type: Note
  time: 52:00
  image: ''
  title: ''
  content: Planar transform node - allows movement and tracking.
  comment: ''
- id: 13
  type: Note
  time: 54:00
  image: ''
  title: ''
  content: >+
    Planar tracker node. you can draw on what you want to track.


    Draw the shape of what to track and click set, to set what to track.


    Planar tracker can be used as a merger node.


    Corner pin mode for screen replacement.

  comment: ''
- id: 14
  type: Note
  time: 01:02:00
  image: ''
  title: ''
  content: When color correcting, if more elements appear corrected then they are supposed to, click the pre-divide/post-multiply check box to clean it up.
  comment: ''
- id: 15
  type: Note
  time: 01:05:00
  image: ''
  title: ''
  content: You can select and group modes together (nto an Underlay) and make them appear as a single node.
  comment: Ctrl+G - Group nodes
- id: 16
  type: Slide
  time: 01:06:00
  image: 1102544836679958528/1102559191903502336.png
  title: ''
  content: diamond symbols on inspector props are where animations live. clicking the diamond sets keyframes with the values selected at the time selected.
  comment: ''
- id: 17
  type: Slide
  time: 01:08:00
  image: 1102544836679958528/1102559701280751616.png
  title: ''
  content: nodes can be attached to multiple outputs for reuse.
  comment: ''
- id: 18
  type: Note
  time: 01:13:30
  image: ''
  title: Quick tips
  content: >-
    * Drag merge node onto a connection inserts it into the connection

    * Shift plus draws can add or remove the node from a connection

    * select a node and add a merge node using Shift+space will insert the merge AFTER the selected node.

    * connect a media node to the output of another node will insert a merge into it.
  comment: ''
content: ''

---
