## Sample 4: Scalable Slideshow with Accessible Templating  
*(PHP)*

A templated slideshow pattern that embeds accessibility, performance, and structural consistency by default, while remaining editable by non-developers. Slide content is defined via a simple data structure, keeping layout logic, interaction behavior, and ARIA relationships centralized and stable.

[Live Example: Before/After Slideshow](https://www.kathycosmeticdentistry.com/#spot04)  
[Full Page](https://www.kathycosmeticdentistry.com/)

### Context
The interface required a content-heavy slideshow that would evolve over time and be maintained by multiple contributors, not all of whom were comfortable editing complex markup.

### Constraint
Slideshows tend to accumulate technical debt quickly: duplicated markup, inconsistent ARIA attributes, fragile tab/tabpanel relationships, and increasing risk of accessibility regressions as content changes.

### Approach
- Define slide content as a simple, ordered data structure.
- Generate all markup, IDs, and relationships programmatically to ensure consistency.
- Centralize ARIA roles, keyboard behavior, and interaction hooks.
- Apply lazy loading and intrinsic sizing to control image cost and layout stability.

### Why this holds up
- Prevents structural drift as slides are added, removed, or reordered.
- Makes accessibility the default rather than an afterthought.
- Allows non-developers to update content without touching interaction logic.
- Scales cleanly without duplicating markup or introducing inconsistencies.

### Representative excerpt
```php
<?php
// Slide content lives in a single, ordered data structure.
// Non-developers only edit this array — not the markup below.
$slides = [
  [
	"topic" => "Topic1",
	"info"  => "...",
	"href"  => "..."
  ],
  [
	"topic" => "Topic2",
	"info"  => "...",
	"href"  => "..."
  ],
  [
	"topic" => "Topic3",
	"info"  => "...",
	"href"  => "..."
  ]
];
?>

<div class="cycle-slideshow"
  data-cycle-timeout="0"
  data-cycle-pager=".spot02pager"
  data-cycle-pager-template='<button role="tab" aria-controls="service{{slideNum}}">{{topic}}</button>'
  data-cycle-slides="> div">

<?php foreach ($slides as $index => $slide):
  $num = $index + 1;
?>
  <div id="service<?= $num ?>" role="tabpanel">
	<h2><?= $slide['topic'] ?></h2>
	<div><?= $slide['info'] ?></div>
	<a href="<?= $slide['href'] ?>">Keep Exploring</a>
  </div>
<?php endforeach; ?>
</div>

<div class="spot02pager" role="tablist"></div>
```

### Notes

This pattern shifts slideshow maintenance from a markup problem to a data-entry task. The primary benefit is not flexibility for its own sake, but reliability: as content changes hands and requirements evolve, the structure remains accessible, predictable, and resistant to regression.
