---
layout: default
title: Product Catalog
---

<h1>{{ page.title }}</h1>

<div class="nav-bar">
  <input
    type="text"
    id="search"
    class="search-bar"
    placeholder="Search products..."
  />
</div>

<div class="catalog">
  <label class="stock-filter-label">
    <input type="checkbox" id="stockFilter" />
    Show only in-stock products
  </label>

  <div class="product-grid">
    {% for product in site.data.product %}
      <div class="product">
        <img class="product-image" src="/assets/images/{{ product.image }}" alt="{{ product.name }}">
        <h2>{{ product.name }}</h2>
        <p>Category: {{ product.category }}</p>
        <p>Price: ₹{{ product.price }}</p>
        <p class="status {% if product.available %}in-stock{% else %}out-stock{% endif %}">
          {% if product.available %}In Stock{% else %}Out of Stock{% endif %}
        </p>

        <p>
          Rating:
          {% assign full_stars = product.rating | floor %}
          {% assign half_star = product.rating | modulo: 1 %}
          {% for i in (1..full_stars) %}★{% endfor %}
          {% if half_star >= 0.5 %}½{% endif %}
          ({{ product.rating }}/5)
        </p>

        <p class="tags">
          {% for tag in product.tags %}
            <span class="tag">{{ tag }}</span>
          {% endfor %}
        </p>

        <button class="buy-button" onclick="alert('Buying {{ product.name }}')">Buy Now</button>
      </div>
    {% endfor %}

  </div> <!-- .product-grid -->
</div> <!-- .catalog -->

<script>
  document.getElementById("search").addEventListener("input", function () {
    const term = this.value.toLowerCase();
    document.querySelectorAll(".product").forEach(function (product) {
      const name = product.querySelector("h2").textContent.toLowerCase();
      const category = product.querySelector("p").textContent.toLowerCase();
      product.style.display =
        name.includes(term) || category.includes(term) ? "" : "none";
    });
  });

  document.getElementById("stockFilter").addEventListener("change", function () {
    const showOnlyInStock = this.checked;
    document.querySelectorAll(".product").forEach(product => {
      const status = product.querySelector(".status").textContent.toLowerCase();
      product.style.display = !showOnlyInStock || status.includes("in stock") ? "" : "none";
    });
  });
</script>
