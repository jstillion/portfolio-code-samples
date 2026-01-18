## Sample 2: Feather Hover Animation  
*(SVG / CSS / JavaScript)*

A small interaction pattern designed to ensure animation completion independent of hover duration. Rather than binding motion directly to hover state, the animation is triggered once and allowed to resolve fully, avoiding abrupt resets during rapid pointer movement.

The goal here is not visual flourish, but interaction predictability.

<a href="https://www.atlantapds.com/" target="_blank" rel="noreferrer noopener">Live Example</a>

### Context
The interface required a lightweight hover affordance that felt deliberate and finished, even when user input was brief or inconsistent.

### Constraint
Hover-driven animations often reset prematurely, producing jittery or incomplete motion that feels accidental rather than intentional — especially during rapid in/out interactions.

### Approach
- Trigger animation via a transient state class rather than continuous hover.
- Allow the animation lifecycle to complete independently of pointer position.
- Keep the interaction purely presentational, with no impact on document semantics or layout.

### Why this holds up
- Prevents partial or interrupted animation states.
- Produces consistent behavior regardless of user input speed.
- Keeps interaction logic reusable and decoupled from markup structure.
- Degrades cleanly if animation is unsupported or removed.

### Evaluation notes
If revisiting this today, I would make several naming and structural refinements:
- Avoid generic class hooks like `.button` in favor of behavior-scoped selectors.
- Use behavior-oriented class names (e.g., `anim-start`) rather than descriptive ones tied to appearance.
- Align keyframe naming with intent rather than inherited project context.

These aren’t functional issues, but they matter for long-term clarity when patterns are reused or handed off.

### Excerpts
---
#### JavaScript
```javascript
document.querySelectorAll('.button').forEach(function(element) {
  element.addEventListener('mouseover', function() {
	this.classList.add('plumage');
  });

  function handleAnimationEnd() {
	this.classList.remove('plumage');
  }

  element.addEventListener('animationend', handleAnimationEnd);
});
```
#### LESS/CSS:
```less
.plumage {
	animation-fill-mode: both;animation-duration:0.8s;
	
	&:before, &:after { content:'';
		  width:3em;height:3em;
		  margin: -1.5em -1.5em;
		  background-position: 50% 50%;
		  background-repeat: no-repeat;
		  background-size: cover;
		  position: absolute;
		  opacity: 0;
		  pointer-events:none;
	}
	&:before{ top: 100%;left: 50%;
		  background-image:url("data:image/svg+xml;charset=UTF-8, ...");
		.omni1;
	}
	&:after{ right: 50%;bottom:100%;
		background-image:url("data:image/svg+xml;charset=UTF-8, ...");
		.omni2;
	}
}
```
#### CSS keyframes:
```css
@keyframes omni1 {
	0% { opacity: 1;transform: scale3d(0.5, 0.5, 1) translate(0,0) rotate(9deg);transition-timing-function:cubic-bezier(0.34, 1.56, 0.64, 1); }
	50% { opacity: 0.5;transform: scale3d(1.1, 1.1, 1) translate(-3em,3em) rotate(-18deg);transition-timing-function:cubic-bezier(0.34, 1.56, 0.64, 1); }
	100% { opacity: 0;transform: scale3d(1.1, 1.1, 1) translate(-3.9em,0) rotate(-180+45deg); }
}
@keyframes omni2 {
	0% { opacity: 1;transform: scale3d(0.5, 0.5, 1) translate(0,0) rotate(-9deg);transition-timing-function:cubic-bezier(0.34, 1.56, 0.64, 1); }
	50% { opacity: 1;transform: scale3d(1.2, 1.2, 1) translate(3em,-3em) rotate(18deg);transition-timing-function:cubic-bezier(0.34, 1.56, 0.64, 1); } 
	100% { opacity: 0;transform: scale3d(1.2, 1.2, 1) translate(3.9em,0) rotate(180+45deg); }
}
.omni1 { animation: omni1 0.5s cubic-bezier(0.61, 1, 0.88, 1) forwards; }//before
.omni2 { animation: omni2 0.5s cubic-bezier(0.61, 1, 0.88, 1) forwards; }//after
```
## Notes:
This pattern has since informed other interactions where completion matters more than responsiveness — including timed visual cues and layered motion sequences. The takeaway is simple: interactions feel intentional when they finish what they start.
