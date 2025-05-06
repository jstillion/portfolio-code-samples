## Sample 2: Feather Hover Animation *(CSS + JavaScript)*

Triggers a lightweight SVG-based animation when the user hovers over a button, ensuring the animation plays through completely—even on rapid hover in/out events. Designed with flexibility and graceful fallback behavior.

[Live Example](atlantapds.com)

### Why it matters:
- Enhances interaction without degrading performance.
- Carefully handles animation timing (via handleAnimationEnd event) to avoid jarring transitions/resetting.
- Easily reusable thanks to querySelectorAll targeting flexibility.
- Allows for semantic HTML separation (animation is purely presentational).

### What it shows:
- Clean, purposeful micro-interactions.
- Reusable logic structure for any similar UI trigger.
- Attention to UX polish via animation lifecycle handling.

"### What I would change about it:
- The class name 'button' as a JavaScript hook? Guilty. It’s a semantic faux pas I wouldn't repeat—I’d go with something more modular and BEM-aligned, like spotbtn--posh.
- Using 'plumage' as the trigger class was a bit too on-the-beak. It describes what the animation looks like, not what it does behaviorally. A more fitting name would be something like animstart or motion-ready—keeping it scoped to behavior, not aesthetics.
- The @keyframes name 'omni' was inherited from a different project where it made contextual sense. For this feather-flipping vibe, something like featherfly1 and featherfly2 would land more semantically.

### Excerpts:
---
#### javascript:
```javascript
document.querySelectorAll('.button').forEach(function(element) {
	
	element.addEventListener('mouseover', function() { this.classList.add('plumage'); });
	function handleAnimationEnd(event) { this.classList.remove('plumage'); }
	
	// Add event listener for different browser prefixes
	element.addEventListener('animationend', handleAnimationEnd);
	element.addEventListener('webkitAnimationEnd', handleAnimationEnd); // For Safari and older Chrome
	element.addEventListener('oAnimationEnd', handleAnimationEnd); // For older Opera

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

## Personal note:
There’s something about animations that complete after a hover event—rather than being bound strictly to the hover state—that really elevates the experience. This approach feels smoother, more intentional. I wish I’d landed on it sooner. The animated birds and their singing that also appear on the [Live Example](atlantapds.com) use similar methods, but I didn’t include those here—as they’re a bit more complex, requiring layered timing to feel natural.
