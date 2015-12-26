title: NASA 3D Printed Wrench
date: 2015-12-26 11:26:02
tags:
---
To test out my new Lulzbot Mini i printed the
[Nasa 3D wrench](http://nasa3d.arc.nasa.gov/detail/wrench-mis).

{% asset_img wrench1.jpg %}

It ratchets in the clockwise direction only but it prints as
a fully functioning part. It's stronger than the rated 3in-lbs.

{% asset_img wrench2.jpg %}

It wasn't all plain sailing though, this being my first ever
3D print, it was a good learning curve.

{% asset_img wrench3.jpg %}

I didn't fully print all of these, i could tell early on if it
was going to work or not.

    | Material | Fillament Dia | Layer Height | Flow | Infill | Result                |
    | ABS      | 2.85mm        | 1.8mm        | 100% | 20%    | Fused solid           |
    | ABS      | 2.85mm        | Std Print    | 100% | 20%    | Fused solid           |
    | ABS      | 3mm           | 2mm          | 90%  | 25%    | Works, slightly loose |
    | ABS      | 3mm           | 1.8mm        | 100  | 25%    | Fused solid           |
    | ABS      | 3mm           | 1.8mm        | 95%  | 25%    | ?                     |

I was having a tough time getting these off the print bed. First
layer adhesion was too good. So i reduced initial layer line width
to 115% which struck a nicer balance of security vs. easier removal.

So it seems my Lulzbot mini over prints very slightly. Upping the
fillament diameter helped but did not fully resolve. I measured the
fillament at between 2.80mm and almost 3mm in some sections.

I measured the infeed and found a 10mm command produced a 9.95mm
feed.

With 20% infill i can't break the handle with hands alone. The
square peg fails easily by splitting a layer. Tolerance of square
peg is tight, even with set to under extrude.
