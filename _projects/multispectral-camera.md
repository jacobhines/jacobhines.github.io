---
title: "Multispectral Camera"
order: 2
excerpt: "Combining visible and thermal imaging for low-cost multispectral sensing<br/><img src='/images/multispectral_thumb.jpg' style='max-width: 500px;'>"
collection: projects
---

Thermal cameras and visible cameras reveal different but complementary information about a scene, and high-resolution thermal cameras are generally expensive compared to their visible counterparts. I built a multispectral camera that captures a high-resolution visible image to enhance a low-resolution thermal image at a fraction of the cost of higher-end thermal cameras.

<img src="/images/multispectral_camera_pipeline.jpg" alt="Multispectral camera pipeline" style="width: 100%; margin-bottom: 2em;">

## Camera Hardware

<style>
.hardware-grid {
  display: grid;
  grid-template-columns: 0.773fr 0.824fr;
  gap: 20px;
  margin: 2em 0;
  align-items: start;
}

@media (max-width: 768px) {
  .hardware-grid {
    grid-template-columns: 1fr;
  }
}
</style>

<div class="hardware-grid">
  <div class="camera-carousel" style="position: relative; display: flex; flex-direction: column;">
    <div class="carousel-container" style="position: relative; overflow: hidden;">
      <img src="/images/camera_iso.jpg" class="carousel-image active" alt="Camera isometric view" style="width: 100%; display: block;">
      <img src="/images/camera_front.jpg" class="carousel-image" alt="Camera front view" style="width: 100%; display: none;">
      <img src="/images/camera_back.jpg" class="carousel-image" alt="Camera back view" style="width: 100%; display: none;">
      <img src="/images/camera_left.jpg" class="carousel-image" alt="Camera left view" style="width: 100%; display: none;">
      <img src="/images/camera_right.jpg" class="carousel-image" alt="Camera right view" style="width: 100%; display: none;">
      <img src="/images/camera_interior.jpg" class="carousel-image" alt="Camera interior" style="width: 100%; display: none;">
    </div>
    <button class="carousel-btn prev" onclick="moveCarousel(-1)" style="position: absolute; top: 50%; left: 10px; transform: translateY(-50%); background: rgba(0,0,0,0.5); color: white; border: none; padding: 10px 15px; cursor: pointer; font-size: 18px; border-radius: 3px; z-index: 10;">&#10094;</button>
    <button class="carousel-btn next" onclick="moveCarousel(1)" style="position: absolute; top: 50%; right: 10px; transform: translateY(-50%); background: rgba(0,0,0,0.5); color: white; border: none; padding: 10px 15px; cursor: pointer; font-size: 18px; border-radius: 3px; z-index: 10;">&#10095;</button>
    <div class="carousel-dots" style="text-align: center; padding: 10px;">
      <span class="dot active" onclick="currentSlide(1)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #717171; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
      <span class="dot" onclick="currentSlide(2)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #bbb; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
      <span class="dot" onclick="currentSlide(3)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #bbb; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
      <span class="dot" onclick="currentSlide(4)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #bbb; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
      <span class="dot" onclick="currentSlide(5)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #bbb; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
      <span class="dot" onclick="currentSlide(6)" style="height: 12px; width: 12px; margin: 0 4px; background-color: #bbb; border-radius: 50%; display: inline-block; cursor: pointer;"></span>
    </div>
  </div>

  <div style="display: flex; flex-direction: column;">
    <img src="/images/camera_vending_machine.jpg" alt="Taking thermal and visible images of a vending machine" style="width: 100%;">
    <p style="font-style: italic; color: #666; margin-top: 0.5em; text-align: center;">Taking thermal and visible images of a vending machine</p>
  </div>
</div>

<script>
let slideIndex = 1;

function moveCarousel(n) {
  showSlide(slideIndex += n);
}

function currentSlide(n) {
  showSlide(slideIndex = n);
}

function showSlide(n) {
  let slides = document.getElementsByClassName("carousel-image");
  let dots = document.getElementsByClassName("dot");

  if (n > slides.length) { slideIndex = 1 }
  if (n < 1) { slideIndex = slides.length }

  for (let i = 0; i < slides.length; i++) {
    slides[i].style.display = "none";
    slides[i].classList.remove("active");
  }
  for (let i = 0; i < dots.length; i++) {
    dots[i].style.backgroundColor = "#bbb";
    dots[i].classList.remove("active");
  }

  slides[slideIndex-1].style.display = "block";
  slides[slideIndex-1].classList.add("active");
  dots[slideIndex-1].style.backgroundColor = "#717171";
  dots[slideIndex-1].classList.add("active");
}
</script>

## Image Registration

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin: 2em 0;">
  <div style="text-align: center;">
    <img src="/images/vending_machine_thermal.jpg" alt="Thermal image" style="width: 100%;">
    <p style="font-style: italic; color: #666; margin-top: 0.5em;">Thermal image</p>
  </div>
  <div style="text-align: center;">
    <img src="/images/vending_machine_visible.jpg" alt="Visible image" style="width: 100%;">
    <p style="font-style: italic; color: #666; margin-top: 0.5em;">Visible image</p>
  </div>
  <div style="text-align: center;">
    <img src="/images/vending_machine_registered.jpg" alt="Registered image pair" style="width: 100%;">
    <p style="font-style: italic; color: #666; margin-top: 0.5em;">Registered image pair</p>
  </div>
</div>

## Image Fusion

Below is a comparison of image fusion techniques across multiple scenes. Columns from left to right: original thermal image, high-pass filter fusion, gradient transfer fusion, and YCbCr color space fusion.

<div style="margin: 2em 0;"> 
  <img src="/images/image_fusion.jpg" alt="Image fusion comparison" style="width: 100%;">
  <p style="font-style: italic; color: #666; margin-top: 0.5em; text-align: center;">Fused images</p>
</div>
