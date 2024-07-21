---
title: Implicit Surface Reconstruction
image: ~/assets/images/cat-recon.png
category: Computer Graphics & Animation
tags:
  - python
  - igl
metadata:
  canonical: https://www.youtube.com/watch?v=mx165chMrPo&t=38s
---

Implemented for UVic Geometric modeling course, tasked with:
- Implementing a implicit MLS (moving-least-squares) function for approximating 3D point cloud
- Sampling implicit function on a 3D volumetric grid
- Apply marching tets algorithm to extract a triangle mesh

Tasked with the construction of an implicit function `f(x)` defined on all `x` R3 whose zero level set contains (or at least passes near) each input point. That is, for every point  `pi`  in the point cloud, we want  `f(pi) = 0`. Furthermore,  `∂f`  (the isosurface normal) evaluated at each point cloud location should approximate the point's normal provided as input.

Construct  `f`  by interpolating a set of target values,  `di`, at "constraint locations",  `ci`. The MLS interpolant is defined by minimization of the form

<img src="https://raw.githubusercontent.com/JonCote/Portfolio/main/src/assets/images/implicit-surface-recon-images/MLS-interp-eq.png">

where `phi(x)` lies in the space of admissible function (e.g., multivariate polynomials up to some degree) and `w` is a weight function that prioritizes each constraint equation depending on the evaluation point, `x`.

## Building the Constraint Equations 

To construct the set of constraint equations each point `pi` in the input point cloud should contribute a constraint with target value `f(pi) = di = 0`. However, these constraints alone provide no information to distinguish the object's inside (where we want `f < 0`) from its outside (where we want `f > 0`). Additionally, the minimization is likely to find the trivial solution `f = 0` (if it lies in the space of admissible functions). To address these problems, we introduce additional constraints incorporating information from the normals as follows:

-   For each point `pi` in the point cloud, add a constraint of the form `f(pi) = di = 0`.
-   Fix an `eps` value, for instance `eps = 0.01 bounding_box_diagonal`.
-   For each point  `pi`  compute  `pi+ = pi + eps ni`, where `ni` is the normalized normal of  `pi`. Check that  `pi`  is the closest point to  `pi+`  if not, halve `esp` and recompute `pi+` until this is the case. Then, add another constraint equation: `f(pi+) = eps`.
-   Repeat the same process for `-eps`, i.e., add equations of the form `f(pi-) = -eps`.
-   Append the tree vectors `pi`, `pi+`, and `pi-` and corresponding `f` to a unique vector `p` and `f`.

After these steps, we have `3n` equations for the implicit function  `f(x)`.

Implementation result given cat point cloud:
<img src="https://raw.githubusercontent.com/JonCote/Portfolio/main/src/assets/images/implicit-surface-recon-images/constraint-sets.png">

The green, red and blue points correspond to inside, outside, and on the surface respectively. The blue points in the right figure are the same as the black points in left figure.

## Praesent tellus ad sapien erat or

- Quam orci nostra mi nulla, hac a.

- Interdum iaculis quis tellus sociis orci nulla, quam rutrum conubia tortor primis.

- Non felis sem placerat aenean duis, ornare turpis nostra.

- Habitasse duis sociis sagittis cursus, ante dictumst commodo.

Duis maecenas massa habitasse inceptos imperdiet scelerisque at condimentum ultrices, nam dui leo enim taciti varius cras habitant pretium rhoncus, ut hac euismod nostra metus sagittis mi aenean. Quam eleifend aliquet litora eget a tempor, ultricies integer vestibulum non felis sodales, eros diam massa libero iaculis.

Nisl ligula ante magnis himenaeos pellentesque orci cras integer urna ut convallis, id phasellus libero est nunc ultrices eget blandit massa ac hac, morbi vulputate quisque tellus feugiat conubia luctus tincidunt curae fermentum. Venenatis dictumst tincidunt senectus vivamus duis dis sociis taciti porta primis, rhoncus ridiculus rutrum curae mattis ullamcorper ac sagittis nascetur curabitur erat, faucibus placerat vulputate eu at habitasse nulla nisl interdum. Varius turpis dignissim montes ac ante tristique quis parturient hendrerit faucibus, consequat auctor penatibus suspendisse rutrum erat nulla inceptos est justo, etiam mollis mauris facilisi cras sociosqu eu sapien sed.

Blandit aptent conubia mollis mauris habitasse suspendisse torquent aenean, ac primis auctor congue cursus mi posuere molestie, velit elementum per feugiat libero dictumst phasellus. Convallis mollis taciti condimentum praesent id porttitor ac dictumst at, sed in eu eleifend vehicula fermentum lectus litora venenatis, gravida hac molestie cum sociosqu mus viverra torquent. Congue est fusce habitasse ridiculus integer suscipit platea volutpat, inceptos varius elementum pellentesque malesuada interdum magnis. Hac lacus eget enim purus massa commodo nec lectus natoque fames arcu, mattis class quam ut neque dui cras quis diam orci sed velit, erat morbi eros suscipit sagittis laoreet vivamus torquent nulla turpis.

Ridiculus velit suscipit consequat auctor interdum magna gravida dictumst libero ut habitasse, sollicitudin vehicula suspendisse leo erat tristique at platea sagittis proin dignissim, id ornare scelerisque et urna maecenas congue tincidunt dictum malesuada. Dui vulputate accumsan scelerisque ridiculus dictum quisque et nam hac, tempus ultricies curabitur proin netus diam vivamus. Vestibulum ante ac auctor mi urna risus lacinia vulputate justo orci sociis dui semper, commodo morbi enim vivamus neque sem pellentesque velit donec hac metus odio. Tempor ultrices himenaeos massa sollicitudin mus conubia scelerisque cubilia, nascetur potenti mauris convallis et lectus gravida egestas sociis, erat eros ultricies aptent congue tortor ornare.

Pretium aliquet sodales aliquam tincidunt litora lectus, erat dui nibh diam mus, sed hendrerit condimentum senectus arcu. Arcu a nibh auctor dapibus eros turpis tempus commodo, libero hendrerit dictum interdum mus class sed scelerisque, sapien dictumst enim magna molestie habitant donec. Fringilla dui sed curabitur commodo varius est vel, viverra primis habitant sapien montes mattis dignissim, gravida cubilia laoreet tempus aliquet senectus. Sociosqu purus praesent porttitor curae sollicitudin accumsan feugiat maecenas donec quis lacus, suscipit taciti convallis odio morbi eros nibh bibendum nunc orci. Magna cras nullam aliquam metus nibh sagittis facilisi tortor nec, mus varius curae ridiculus fames congue interdum erat urna, neque odio lobortis mi mattis diam cubilia arcu.

Laoreet fusce nec class porttitor mus proin aenean, velit vestibulum feugiat porta egestas sapien posuere, conubia nisi tempus varius hendrerit tortor. Congue aliquam scelerisque neque vivamus habitasse semper mauris pellentesque accumsan posuere, suspendisse lectus gravida erat sagittis arcu praesent mus ornare. Habitasse nibh nam morbi mollis senectus erat risus, cum sollicitudin class platea congue mattis venenatis, luctus aenean parturient hendrerit malesuada ante. Mus auctor tincidunt consequat massa tortor nulla luctus habitasse vestibulum quis velit, laoreet sagittis cum facilisi in sem tellus leo vulputate vehicula bibendum orci, felis nisl blandit lacus convallis congue turpis magna facilisis condimentum.

Dictumst pellentesque urna donec sociis suscipit montes consequat, commodo quam habitasse senectus fringilla maecenas, inceptos magna tristique eu nullam nam. Maecenas orci nibh hac eu tristique ut penatibus ultrices ante, pellentesque cubilia pharetra dis facilisis aliquam praesent malesuada vivamus, commodo cras velit convallis molestie nec tellus augue. Etiam ut convallis risus id dapibus platea laoreet accumsan, habitant et aenean netus inceptos iaculis per, mauris curae at ligula odio ad eu. Mauris erat tempor interdum sapien commodo per nullam tortor, fusce facilisis vehicula egestas dui nulla conubia ut fames, fringilla et tincidunt penatibus facilisi at mollis.

Fermentum sociosqu litora primis sollicitudin fusce diam consequat vehicula per lobortis et, viverra sodales magna rutrum sed mollis faucibus molestie purus montes est, risus nostra congue venenatis lectus enim torquent eros dis dapibus. Dui suscipit scelerisque massa ligula euismod accumsan augue, magna vel lacus ante nullam senectus commodo, viverra cubilia eros eget penatibus tempor. Mattis mauris hac felis semper dui sociis faucibus mollis ornare pretium aliquam velit nisl, quis litora sem at vel duis rutrum imperdiet natoque viverra himenaeos tempor.

Integer eu tristique purus luctus vivamus porttitor vel nisl, tortor malesuada augue vulputate diam velit pellentesque sodales, duis phasellus vestibulum fermentum leo facilisi porta. Hac porttitor cum dapibus volutpat quisque odio taciti nulla senectus mollis curae, accumsan suscipit cubilia tempor ligula in venenatis justo leo erat, magna tincidunt nullam lacinia luctus malesuada non vivamus praesent pharetra. Non quam felis montes pretium volutpat suspendisse lacus, torquent magna dictumst orci libero porta, feugiat taciti cras ridiculus aenean rutrum. Tellus nostra tincidunt hac in ligula mi vulputate venenatis pellentesque urna dui, at luctus tristique quisque vel a dignissim scelerisque platea pretium, suspendisse ante phasellus porttitor quis aliquam malesuada etiam enim nullam.

Hendrerit taciti litora nec facilisis diam vehicula magnis potenti, parturient velit egestas nisl lobortis tincidunt rutrum cursus, fusce senectus mi massa primis mattis rhoncus. Accumsan est ac varius consequat vulputate, ligula cursus euismod sagittis inceptos scelerisque, lacus malesuada torquent dictumst. Volutpat morbi metus urna rhoncus nunc tempor molestie, congue curabitur quis interdum posuere. Mollis viverra velit tortor mus netus nunc molestie metus, sem massa himenaeos luctus feugiat taciti iaculis fames porttitor, leo arcu consequat gravida dapibus pulvinar elementum.
