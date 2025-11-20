<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Digital Wedding Invitation</title>
<style>
  /* GENERAL PAGE STYLING */
  body {
    margin: 0;
    font-family: sans-serif;
    background: #f7f7f7;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* SLIDER CONTAINER */
  .slider-container {
    width: 100%;
    overflow: hidden;
    position: relative;
    margin: 20px 0;
  }

  .slider {
    display: flex;
    transition: transform 0.5s ease;
  }

  .slide {
    min-width: 100%;
    user-select: none;
  }

  .slide img {
    width: 80%;       
    max-width: 600px; 
    height: auto;     
    display: block;
    margin: 0 auto;  
    border-radius: 8px; 
    box-shadow: 0 4px 8px rgba(0,0,0,0.1); 
    object-fit: cover;
  }

  /* DOTS UNDER SLIDER */
  .dots {
    text-align: center;
    padding: 10px 0;
  }
  .dot {
    height: 10px;
    width: 10px;
    margin: 3px;
    display: inline-block;
    background: #bbb;
    border-radius: 50%;
  }
  .dot.active {
    background: #555;
  }

  /* STATIC IMAGES ABOVE AND BELOW SLIDER */
  .static-image-top img,
  .static-image-bottom img {
    width: 80%;
    max-width: 600px;
    height: auto;
    display: block;
    margin: 20px auto;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    object-fit: cover;
  }

  /* WEDDING INVITATION TILE */
  .invite-tile {
    width: 90%;
    max-width: 400px;
    padding: 20px;
    margin: 20px auto 40px auto;
    background: linear-gradient(135deg, #f8e1e7, #fce4ec);
    border-radius: 12px;
    text-align: center;
    font-family: 'Georgia', serif;
    font-size: 1.2em;
    color: #d6336c;
    cursor: pointer;
    box-shadow: 0 6px 12px rgba(0,0,0,0.1);
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .invite-tile:hover {
    transform: translateY(-4px);
    box-shadow: 0 10px 18px rgba(0,0,0,0.15);
  }
</style>
</head>
<body>

<!-- STATIC IMAGE 1 ABOVE THE SLIDER -->
<div class="static-image-top">
  <img src="1.png" alt="Top Image">
</div>

<!-- AUTO-SLIDER -->
<div class="slider-container">
  <div class="slider" id="slider">
    <div class="slide"><img src="image1.png"></div>
    <div class="slide"><img src="image2.png"></div>
    <div class="slide"><img src="image3.png"></div>
  </div>
</div>
<div class="dots" id="dots"></div>

<!-- STATIC IMAGE 2 BELOW THE SLIDER -->
<div class="static-image-bottom">
  <img src="2.png" alt="Bottom Image">
</div>

<!-- WEDDING INVITATION TILE BELOW IMAGE 2 -->
<div class="invite-tile" onclick="openInvitation()">
  <h2>💌 Open Our Wedding Invitation</h2>
</div>

<!-- SCRIPTS -->
<script>
  const slider = document.getElementById("slider");
  const slides = document.querySelectorAll(".slide");
  const dots = document.getElementById("dots");

  let index = 0, startX = 0, dragging = false;

  // Create dots
  slides.forEach((_, i) => {
    const d = document.createElement("div");
    d.classList.add("dot");
    if (i === 0) d.classList.add("active");
    dots.appendChild(d);
  });

  const updateDots = () => {
    document.querySelectorAll(".dot").forEach((dot, i) => {
      dot.classList.toggle("active", i === index);
    });
  };

  const goToSlide = (i) => {
    index = (i + slides.length) % slides.length;
    slider.style.transform = `translateX(${-index * 100}%)`;
    updateDots();
  };

  // Auto-swipe every 5 seconds
  setInterval(() => {
    goToSlide(index + 1);
  }, 5000);

  // Touch swipe for mobile
  slider.addEventListener("touchstart", e => {
    dragging = true;
    startX = e.touches[0].clientX;
  });

  slider.addEventListener("touchmove", e => {
    if (!dragging) return;
    const diff = e.touches[0].clientX - startX;
    slider.style.transform = `translateX(${diff - index * window.innerWidth}px)`;
  });

  slider.addEventListener("touchend", e => {
    dragging = false;
    const diff = e.changedTouches[0].clientX - startX;
    if (diff < -50) goToSlide(index + 1);
    else if (diff > 50) goToSlide(index - 1);
    else goToSlide(index);
  });

  // Invitation tile click
  function openInvitation() {
    const sliderSection = document.querySelector('.slider-container');
    sliderSection.scrollIntoView({ behavior: 'smooth' });
  }
</script>

</body>
</html>
