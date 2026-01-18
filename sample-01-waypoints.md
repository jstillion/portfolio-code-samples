## Sample 1: Scroll-Activated State Triggers  *(JavaScript / Waypoints)*

A reusable pattern for managing scroll-based state changes without relying on timing assumptions or tightly coupled animation logic. The approach toggles simple attributes based on viewport position, allowing animations and behaviors to remain declarative and resilient.

The implementation supports development-time iteration while allowing production behavior to be locked down, reducing the risk of inconsistent animation states at launch.

[Live Example](https://www.ranjbarorthodontics.com/)

### Context
Homepage sections required staged, scroll-triggered behavior that remained predictable across devices and avoided premature or missed animation states.

### Constraint
Scroll-driven interactions are easy to make visually appealing but notoriously brittle when tied directly to animation timing, rapid scroll direction changes, or device-specific behavior.

### Approach
- Use a lightweight scroll observer (Waypoints) to toggle semantic attributes based on scroll direction and offset.
- Separate development and production behavior to allow safe iteration without reworking logic.
- Gate execution by viewport width to avoid unnecessary work on smaller devices.

### Why this holds up
- Avoids animation timing assumptions.
- Keeps state management explicit and reversible.
- Allows animations to opt in without owning scroll logic.
- Adapts easily to new sections or reuse elsewhere.

### Representative excerpt
```javascript
document.addEventListener('DOMContentLoaded', function() {
	let mql = window.matchMedia("(min-width: 1300px)");

	if (mql.matches) {  // Check if the media query matches
		
		function waytrig(id, className, jumpoff) {
		  var waypoint = new Waypoint({
			element: document.getElementById(id),
			handler: function(direction) {
			  //$('.' + className).removeAttr('data-ready');//<< uncomment for single trigger on live site 
			  if (direction == 'down') { $('.' + className).removeAttr('data-ready'); }
			  if (direction == 'up') { $('.' + className).attr('data-ready', ''); }
			},
			offset: jumpoff
		  });
		}
		
		  waytrig('spot01', 'spot01', '48%');
		  waytrig('spot02', 'spot02', '48%');
		  waytrig('bd', 'ornament', '48%');
		  waytrig('spot03', 'spot03', '75%');
		  waytrig('ft', 'marker', '80%');
		
	}
});
```

## 
Notes:
This same pattern has been reused for deferring heavy assets, pausing CPU-intensive components when off-screen, and triggering state changes unrelated to animation. The core value is not the effect itself, but the predictability of when and why state changes occur.
