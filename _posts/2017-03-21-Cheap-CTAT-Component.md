---
layout: post
title: "Cheap Way to Make a Custom CTAT component"
author: Michael Ringenberg
date: 2017-03-21
---
I started working on a new interface for a Causal Models
[CTAT](https://github.com/CMUCTAT/CTAT/) tutor in which
students specify relations between variables using a graph drawing tool:
[Cytoscape](http://js.cytoscape.org/) with
[Cytoscape.js-edgehandles](https://github.com/cytoscape/cytoscape.js-edgehandles).
Having developed a couple of custom components at this point, I know
that it is non-trivial to code one from scratch so in order to
prototype the interaction before committing to developing it as a full
custom component I ended up using a bit of custom JavaScript in order
to fake things a bit before getting the go ahead to develop another
custom component. Fortunately, this little trick seems to be working
out well enough that I might not need to fully develop a custom
component for this task. I also figured that other people working with
[CTAT](https://github.com/CMUCTAT/CTAT/) might benefit from this trick.

One caveat to note is that the grain size of the grading event for
this new component is on the current state of the graph, not on each
student action within the component. In other words, it is closer to a
check box group or a pie chart with a submit button rather than a
button or a combo box. Fortunately, we designed CTAT in order to
handle various grain sizes of interaction, even though it works best
with the finer grain size of student interactions.

The trick is to use some JavaScript attached as event handlers to the
graph object that update the value attribute of a CTATButton
component. Typically, when using a CTATButton, the Input of the
Selection-Action-Input is usually ignored as irrelevant and defaults
to `-1`.  But, because it was useful to use CTATButton's in
conjunction with complex components like the CTATNumberLine, see the
Controlling tick marks example on the
[CTATNumberLine examples](https://cdn.rawgit.com/CMUCTAT/CTAT/latest/Examples/CTATNumberLine.html),
having a way to specify the value was necessary. As per the
[CTATButton](https://github.com/CMUCTAT/CTAT/wiki/CTATButton)
documentation, there is a value attribute. What may not be clear is
that the CTATButton retrieves that value every time it is pressed and
is not stored or cached internally. This means that it is possible to
update the `value` attribute each time there is a change in the graph
to some sort of representation of the state, which in this case is a
list of edges.  When the CTATButton is pressed, it will then send this
state information as the Input which can be examine by CTAT for
correctness.

At this point, only the CTATButton will show any corrective feedback,
but that is also easily extendable to the graph by adding event
handlers to the button component that reacts to grading events on it.
For example:
```javascript
$('#SubmitRelationGraph').on('CTAT_CORRECT', function () {
    $('#RelationshipGraph').removeClass('CTAT--hint CTAT--incorrect').addClass('CTAT--correct');
}).on('CTAT_INCORRECT', function () {
    $('#RelationshipGraph').removeClass('CTAT--hint CTAT--correct').addClass('CTAT--incorrect');
}).on('CTAT_HIGHLIGHT', function (ev) {
    if (ev.detail.isHighlighted) { // CTAT_HIGHLIGHT is triggered on both highlight and unhighlight, therefore check which it is.
        $('#RelationshipGraph').removeClass('CTAT--correct CTAT--incorrect').addClass('CTAT--hint');
    } else {
        $('#RelationshipGraph').removeClass('CTAT--hint');
    }
}).on('CTAT_NOTGRADED', function () {

    $('#RelationshipGraph').removeClass('CTAT--hint CTAT--correct CTAT--incorrect');
});
```

The CTAT events that are used in grading are:
* ```"CTAT_CORRECT"```
* ```"CTAT_INCORRECT"```
* ```"CTAT_HIGHLIGHT"```
* ```"CTAT_NOTGRADED"```

The CTAT css classes used in grading are:
* ```CTAT--correct```
* ```CTAT--incorrect```
* ```CTAT--hint```
