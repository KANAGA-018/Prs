#prs
<!DOCTYPE html>
<html>
<head>
    <title>AI Based Product Recommendation System</title>
    <style>
        body { font-family: Arial; margin:0; background:#f3f4f6; }
        header { background:#2563eb; color:white; padding:15px; text-align:center; }
        .container { width:95%; margin:auto; margin-top:20px; }
        .section { background:white; padding:15px; border-radius:10px; margin-bottom:20px; }
        input, select { padding:8px; width:100%; margin:5px 0; }
        button { padding:8px; background:#2563eb; color:white; border:none; border-radius:5px; cursor:pointer; }
        .products { display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:15px; }
        .card { background:white; padding:10px; border-radius:10px; box-shadow:0 0 8px rgba(0,0,0,0.1); text-align:center; }
        .card img { width:100%; height:160px; object-fit:cover; }
        .rating { color:orange; }
    </style>
</head>
<body>

<header>
    <h1>AI Based Product Recommendation System</h1>
</header>

<div class="container">

<div class="section">
    <h2>Search Product</h2>
    <input type="text" id="search" placeholder="Search product...">
    <button onclick="searchProduct()">Search</button>
</div>

<div class="section">
    <h2>Recommended For You 🤖</h2>
    <div class="products" id="recommended"></div>
</div>

<div class="section">
    <h2>All Products</h2>
    <div class="products" id="allProducts"></div>
</div>

</div>

<script>

let userPreference = {
    categoryClicks: {},
    searchHistory: []
};

const products = [
{name:"iPhone 14",category:"mobile",rating:4.8,price:75000,image:"https://m.media-amazon.com/images/I/61cwywLZR-L._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Samsung S23",category:"mobile",rating:4.5,price:65000,image:"https://m.media-amazon.com/images/I/61VfL-aiToL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"HP Pavilion",category:"laptop",rating:4.3,price:58000,image:"https://m.media-amazon.com/images/I/71vFKBpKakL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Dell Inspiron",category:"laptop",rating:4.2,price:55000,image:"https://m.media-amazon.com/images/I/71hX8cB+YgL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Nike Air Max",category:"shoes",rating:4.6,price:8000,image:"https://static.nike.com/a/images/t_default/...",link:"https://www.flipkart.com/"},
{name:"Boat Smart Watch",category:"watch",rating:4.0,price:3000,image:"https://m.media-amazon.com/images/I/61IMRs+o0iL._SL1500_.jpg",link:"https://www.amazon.in/"}
];

function displayProducts(list, elementId){
    const container=document.getElementById(elementId);
    container.innerHTML="";
    list.forEach(p=>{
        container.innerHTML+=`
        <div class="card" onclick="trackClick('${p.category}')">
            <img src="${p.image}">
            <h3>${p.name}</h3>
            <p><b>₹${p.price}</b></p>
            <p class="rating">${"⭐".repeat(Math.floor(p.rating))}</p>
            <button onclick="window.open('${p.link}')">Buy</button>
        </div>`;
    });
}

function searchProduct(){
    let search=document.getElementById("search").value.toLowerCase();
    userPreference.searchHistory.push(search);

    let filtered=products.filter(p=>p.name.toLowerCase().includes(search));
    displayProducts(filtered,"allProducts");
    generateAIRecommendations();
}

function trackClick(category){
    if(!userPreference.categoryClicks[category]){
        userPreference.categoryClicks[category]=0;
    }
    userPreference.categoryClicks[category]++;
    generateAIRecommendations();
}

function generateAIRecommendations(){

    let scoredProducts=products.map(p=>{
        let score=0;

        // Higher rating = higher score
        score+=p.rating*2;

        // If user clicked same category before
        if(userPreference.categoryClicks[p.category]){
            score+=userPreference.categoryClicks[p.category]*5;
        }

        // If product matches search history
        userPreference.searchHistory.forEach(s=>{
            if(p.name.toLowerCase().includes(s)){
                score+=10;
            }
        });

        return {...p,score:score};
    });

    scoredProducts.sort((a,b)=>b.score-a.score);

    let top=scoredProducts.slice(0,4);
    displayProducts(top,"recommended");
}

displayProducts(products,"allProducts");
generateAIRecommendations();

</script>

</body>
</html>
