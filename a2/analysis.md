% CSC321: Assignment 2
% Zeeshan Qureshi (997108954)
% 6 Mar 2013

Programming
===========

Initially I did not want to look at the post on the forum and derive the
gradients from scratch but the relatively large amount of variables
compared to the example done in class caused a lot of confusion. After
looking at the example on the forum, the derivation wasn't particularly
hard but deciding on a consistent naming scheme was the key. After using
greek alphabets for weight matrices and capital letters for Input/Output
ones and writing down the indices used for each one of them it became
pretty clear what was going on.

These are the steps I followed in the gradient derivation:

  + Figure out the formula for the error E from the loss function
  + Figure out the chain of derivatives leading from E to the
    weights from hidden units -> classes
  + Similarly figure out the chain of derivatives from E to the
    weights from input -> hidden units

Once I had the formulas down for each unit, I vectorized the formula as in
the forum post for better performance.

Gradient Code
-------------

    ret.input_to_hid = (((model.hid_to_class' * (class_prob - data.targets))
        .* hid_output .* (1 - hid_output)) * data.inputs')
        / size(data.targets, 2);

    ret.hid_to_class = ((class_prob - data.targets) * hid_output')
        / size(data.targets, 2);

Result
------

After implementing the code and running *a2(0, 10, 30, 0.01, 0, false, 10)*
the training data classification loss was **2.301907**.

Optimization
============

Learning Rate   Momentum = 0    Momentum = 0.9
-------------  --------------  ----------------
0.002           2.302293        2.299567
0.01            2.300903        2.285062
0.05            2.293413        1.994052
0.2             2.223583        **1.292496**
1.0             **1.674933**    1.877051
5.0             2.301613        2.302585
20.0            2.302583        2.302585

Table: Optimization Results

Without the momentum the error rate goes down steadily but with momentum
and larger learning rates (>= 1.0), the error rate seems
to wobble around towards the end which means we could get into the
problem of oscillating around the optimum if the momentum and learning
rate are too high.
