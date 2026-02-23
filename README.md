#Prs
<!DOCTYPE html>
<html>
<head>
    <title>Advanced Product Recommendation System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background: #f3f4f6;
        }

        header {
            background: linear-gradient(to right, #2563eb, #1e40af);
            color: white;
            padding: 15px;
            text-align: center;
        }

        .container {
            width: 95%;
            margin: auto;
            margin-top: 20px;
        }

        .filters {
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }

        select, input {
            padding: 8px;
            margin: 5px 0;
            width: 100%;
        }

        button {
            padding: 10px;
            width: 100%;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background: #1e40af;
        }

        .products {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
            gap: 15px;
        }

        .card {
            background: white;
            border-radius: 10px;
            padding: 10px;
            box-shadow: 0 0 8px rgba(0,0,0,0.1);
            text-align: center;
        }

        .card img {
            width: 100%;
            height: 160px;
            object-fit: cover;
        }

        .buy-btn {
            background: green;
            margin-top: 8px;
        }

        .rating {
            color: orange;
        }

    </style>
</head>
<body>

<header>
    <h1>Advanced Product Recommendation System</h1>
</header>

<div class="container">

    <div class="filters">
        <h2>Customize & Filter Products</h2>

        <input type="text" id="search" placeholder="Search product...">

        <label>Category</label>
        <select id="category">
            <option value="all">All</option>
            <option value="mobile">Mobile</option>
            <option value="laptop">Laptop</option>
            <option value="shoes">Shoes</option>
        </select>

        <label>Brand</label>
        <select id="brand">
            <option value="all">All</option>
            <option value="Apple">Apple</option>
            <option value="Samsung">Samsung</option>
            <option value="HP">HP</option>
            <option value="Nike">Nike</option>
        </select>

        <label>Minimum Rating</label>
        <select id="rating">
            <option value="0">All</option>
            <option value="3">3⭐ & above</option>
            <option value="4">4⭐ & above</option>
        </select>

        <button onclick="filterProducts()">Find Products</button>
    </div>

    <div class="products" id="productList"></div>

</div>

<script>

const products = [
    {name:"iPhone 14", category:"mobile", brand:"Apple", rating:4.8, price:75000, image:"https://m.media-amazon.com/images/I/61cwywLZR-L._SL1500_.jpg", link:"https://www.amazon.in/"},
    {name:"Samsung Galaxy S23", category:"mobile", brand:"Samsung", rating:4.5, price:65000, image:"https://m.media-amazon.com/images/I/61VfL-aiToL._SL1500_.jpg", link:"https://www.amazon.in/"},
    {name:"HP Pavilion 15", category:"laptop", brand:"HP", rating:4.3, price:58000, image:"https://m.media-amazon.com/images/I/71vFKBpKakL._SL1500_.jpg", link:"https://www.amazon.in/"},
    {name:"Nike Air Max", category:"shoes", brand:"Nike", rating:4.6, price:8000, image:"https://static.nike.com/a/images/t_default/...", link:"https://www.flipkart.com/"}
];

function displayProducts(list) {
    const container = document.getElementById("productList");
    container.innerHTML = "";

    if(list.length === 0){
        container.innerHTML = "<h3>No Products Found</h3>";
        return;
    }

    list.forEach(product => {
        container.innerHTML += `
            <div class="card">
                <img src="${product.image}" alt="${product.name}">
                <h3>${product.name}</h3>
                <p><b>Brand:</b> ${product.brand}</p>
                <p><b>Price:</b> ₹${product.price}</p>
                <p class="rating">${"⭐".repeat(Math.floor(product.rating))} (${product.rating})</p>
                <button class="buy-btn" onclick="window.open('${product.link}')">Buy Now</button>
            </div>
        `;
    });
}

function filterProducts() {
    const search = document.getElementById("search").value.toLowerCase();
    const category = document.getElementById("category").value;
    const brand = document.getElementById("brand").value;
    const rating = parseFloat(document.getElementById("rating").value);

    const filtered = products.filter(p =>
        (p.name.toLowerCase().includes(search)) &&
        (category === "all" || p.category === category) &&
        (brand === "all" || p.brand === brand) &&
        (p.rating >= rating)
    );

    displayProducts(filtered);
}

displayProducts(products);

</script>

</body>
</html>
