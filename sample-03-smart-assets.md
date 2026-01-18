## Sample 3: Smart Asset Loading  
*(PHP)*

A defensive asset-loading pattern that conditionally includes scripts and styles only on pages that require them, avoiding unnecessary global payload and reducing performance overhead across the rest of the site.

There’s no visible demo for this example — the value is structural rather than visual.

### Context
The site relied on globally loaded scripts and styles that were only required on a small number of rarely visited pages. Despite limited use, these assets were being shipped on every request.

### Constraint
Globally managed assets are convenient, but they silently accumulate cost: increased payload, slower page loads, and degraded performance metrics across the entire site.

### Approach
- Gate asset inclusion by request context rather than global configuration.
- Ensure helper functions execute only when explicitly invoked.
- Keep conditional logic centralized and easy to extend as new requirements emerge.

### Why this holds up
- Reduces unnecessary network and parsing cost on the majority of pages.
- Scales cleanly as additional conditional assets are introduced.
- Limits side effects by keeping execution opt-in rather than implicit.
- Improves performance without altering templates or editorial workflows.

### Representative excerpt (helper)
```php
if (!function_exists('load_abc')) {
  function load_abc($type, $vendor) {
	$valid_paths = ['/pagename1', '/pagename2', '/pagename3'];
	foreach ($valid_paths as $path) {
	  if (strpos($_SERVER['REQUEST_URI'], $path) !== false) {
		if ($type === 'styles') {
		  echo '<link rel="stylesheet" href="' . $vendor . 'ABC/abc.min.css">';
		} elseif ($type === 'script') {
		  echo '<script src="' . $vendor . 'ABC/abc.min.js"></script>';
		}
		return;
	  }
	}
  }
}
```

### Representative excerpt (layout)
```php
<head>
  <link rel="stylesheet" href="<?php echo $vendor . $global_css; ?>">
  <link rel="stylesheet" href="/css/local.css">
  <?php load_abc('styles', $vendor); ?>
  <?php load_xyz($vendor); ?>
</head>
<body>
  ...
  <script src="<?php echo $vendor . $global_js; ?>"></script>
  <script defer src="/js/local.js"></script>
  <?php load_abc('script', $vendor); ?>
</body>
```

### Notes
This change wasn’t visible to users, but it meaningfully reduced baseline page weight and improved performance metrics sitewide. It’s a reminder that some of the most impactful front-end work happens quietly, by preventing cost rather than adding features.
