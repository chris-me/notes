# Call-To-Action Top Bar

- Create Child Theme
- Copy / modify header.php (right before `<div id="et-secondary-menu">`) and insert:

```php
<!-- BEGIN SECONDARY MENU alias CALL TO ACTION TOP BAR -->

<div class="cta">
  <div class="cta-icon">
    <i class="fas fa-2x fa-phone"></i>
  </div>
  <div class="cta-phone">
    <div>
      Sales department
    </div>
    <div>
      0123 / 456789-10
    </div>
  </div>
  <div class="cta-icon">
    <i class="fas fa-2x fa-clipboard-list"></i>
  </div>
  <div>
    <div>
      Request a Quote
    </div>
    <div>
      Rapid response.<a href='#'> Learn more.</a>
    </div>
  </div>
</div>

<!-- END SECONDARY MENU alias CALL TO ACTION TOP BAR -->
```

- style.css

```
/* BEGIN SECONDARY MENU alias CALL TO ACTION TOP BAR */

.cta {
  display: flex;
  justify-content: center;
  transition: 0.3s;
}

.cta .cta-phone {
  margin-right: 20px;
}

.cta .cta-icon {
  margin-right: 10px;
}

.cta:hover {
  color: #003163;
}

/* END SECONDARY MENU alias CALL TO ACTION TOP BAR */
```
