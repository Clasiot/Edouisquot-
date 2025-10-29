 <!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fiverr Clone Interactive</title>
<style>
  body {margin:0; font-family:Arial,sans-serif; background:#f5f5f5;}
  header {background:#1dbf73; color:white; padding:20px; text-align:center;}
  nav {display:flex; justify-content:center; flex-wrap:wrap; gap:10px; background:#fff; padding:10px 0; box-shadow:0 2px 5px rgba(0,0,0,0.1);}
  nav button {border:none; background:#eee; padding:8px 12px; border-radius:5px; cursor:pointer;}
  nav button.active, nav button:hover {background:#1dbf73; color:white;}
  .search-bar {text-align:center; margin:20px 0;}
  .search-bar input {width:60%; padding:10px; font-size:16px; border-radius:5px; border:1px solid #ccc;}
  .services {display:flex; flex-wrap:wrap; justify-content:center; gap:20px; padding:20px;}
  .service-card {background:white; border-radius:8px; width:250px; padding:15px; box-shadow:0 2px 5px rgba(0,0,0,0.1); text-align:center; transition: transform 0.2s, box-shadow 0.2s;}
  .service-card:hover {transform:translateY(-5px); box-shadow:0 5px 15px rgba(0,0,0,0.2);}
  .service-card img {width:100%; border-radius:8px;}
  .service-card h3 {margin:10px 0 5px;}
  .service-card p {font-size:14px; color:#555;}
  .price {font-weight:bold; margin:10px 0;}
  .service-card button {margin-top:10px; padding:10px 20px; background:#1dbf73; color:white; border:none; border-radius:4px; cursor:pointer; font-weight:bold;}
  .service-card button:hover {background:#17a159;}
  /* Popup modal */
  .modal {display:none; position:fixed; z-index:10; left:0; top:0; width:100%; height:100%; background:rgba(0,0,0,0.5); justify-content:center; align-items:center;}
  .modal-content {background:white; padding:20px; border-radius:8px; width:90%; max-width:400px; text-align:center; position:relative;}
  .close {position:absolute; top:10px; right:15px; cursor:pointer; font-size:18px; font-weight:bold;}
  @media(max-width:600px){.search-bar input{width:90%;} nav{flex-direction:column;}}
</style>
</head>
<body>

<header>
  <h1>Fiverr Clone Interactive</h1>
  <p>Find the perfect freelance service for your business</p>
</header>

<nav>
  <button class="category-btn active" data-category="all">Tous</button>
  <button class="category-btn" data-category="design">Graphic & Design</button>
  <button class="category-btn" data-category="marketing">Digital Marketing</button>
  <button class="category-btn" data-category="writing">Writing & Translation</button>
  <button class="category-btn" data-category="video">Video & Animation</button>
  <button class="category-btn" data-category="tech">Programming & Tech</button>
</nav>

<div class="search-bar">
  <input type="text" placeholder="Rechercher un service...">
</div>

<div class="services">
  <div class="service-card" data-category="design">
    <img src="https://via.placeholder.com/250x150" alt="Logo Design">
    <h3>Logo Design</h3>
    <p>Professional custom logo design</p>
    <div class="price">$50</div>
    <button>Commander</button>
  </div>
  <div class="service-card" data-category="marketing">
    <img src="https://via.placeholder.com/250x150" alt="SEO Optimization">
    <h3>SEO Optimization</h3>
    <p>Improve your website ranking on Google</p>
    <div class="price">$80</div>
    <button>Commander</button>
  </div>
  <div class="service-card" data-category="tech">
    <img src="https://via.placeholder.com/250x150" alt="Website Development">
    <h3>Website Development</h3>
    <p>Responsive and modern website</p>
    <div class="price">$150</div>
    <button>Commander</button>
  </div>
  <div class="service-card" data-category="video">
    <img src="https://via.placeholder.com/250x150" alt="Video Editing">
    <h3>Video Editing</h3>
    <p>Professional video editing</p>
    <div class="price">$100</div>
    <button>Commander</button>
  </div>
  <div class="service-card" data-category="writing">
    <img src="https://via.placeholder.com/250x150" alt="Content Writing">
    <h3>Content Writing</h3>
    <p>High-quality blog and article writing</p>
    <div class="price">$40</div>
    <button>Commander</button>
  </div>
  <div class="service-card" data-category="marketing">
    <img src="https://via.placeholder.com/250x150" alt="Social Media Marketing">
    <h3>Social Media Marketing</h3>
    <p>Grow your social media presence professionally</p>
    <div class="price">$70</div>
    <button>Commander</button>
  </div>
</div>

<!-- Modal -->
<div class="modal" id="orderModal">
  <div class="modal-content">
    <span class="close">&times;</span>
    <h3>Commander Service</h3>
    <p id="serviceName"></p>
    <p>Prix: <span id="servicePrice"></span></p>
    <input type="text" placeholder="Votre nom" id="clientName" style="width:90%;padding:8px;margin:5px 0;"><br>
    <input type="email" placeholder="Votre email" id="clientEmail" style="width:90%;padding:8px;margin:5px 0;"><br>
    <button onclick="confirmOrder()">Confirmer la commande</button>
  </div>
</div>

<script>
  // Search
  const searchInput = document.querySelector('.search-bar input');
  const cards = document.querySelectorAll('.service-card');
  searchInput.addEventListener('input', () => {
    const filter = searchInput.value.toLowerCase();
    cards.forEach(card => {
      const title = card.querySelector('h3').textContent.toLowerCase();
      const desc = card.querySelector('p').textContent.toLowerCase();
      if(title.includes(filter) || desc.includes(filter)) {
        card.style.display = 'block';
      } else {
        card.style.display = 'none';
      }
    });
  });

  // Filter categories
  const categoryBtns = document.querySelectorAll('.category-btn');
  categoryBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      categoryBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const category = btn.dataset.category;
      cards.forEach(card => {
        if(category === 'all' || card.dataset.category === category){
          card.style.display = 'block';
        } else {
          card.style.display = 'none';
        }
      });
    });
  });

  // Modal
  const modal = document.getElementById('orderModal');
  const serviceName = document.getElementById('serviceName');
  const servicePrice = document.getElementById('servicePrice');
  const closeModal = document.querySelector('.close');

  cards.forEach(card => {
    card.querySelector('button').addEventListener('click', () => {
      serviceName.textContent = card.querySelector('h3').textContent;
      servicePrice.textContent = card.querySelector('.price').textContent;
      modal.style.display = 'flex';
    });
  });

  closeModal.addEventListener('click', () => { modal.style.display = 'none'; });

  window.onclick = function(event) { if(event.target == modal){ modal.style.display='none'; }}

  function confirmOrder() {
    const name = document.getElementById('clientName').value;
    const email = document.getElementById('clientEmail').value;
    if(name && email){
      alert(`Commande confirmée pour ${serviceName.textContent} !\nNom: ${name}\nEmail: ${email}`);
      modal.style.display = 'none';
      document.getElementById('clientName').value = '';
      document.getElementById('clientEmail').value = '';
    } else {
      alert('Veuillez remplir votre nom et email.');
    }
  }
</script>

</body>
</html>
