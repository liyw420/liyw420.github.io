---
layout: project_page
permalink: /

title: "Geometry-Consistent 4D Gaussian Splatting for Sparse-Input Dynamic View Synthesis"
authors: "<a href='https://scholar.google.com/citations?user=RYUlClcAAAAJ&hl=en'>Yiwei Li</a><sup>†</sup>, <a href='https://www4.comp.polyu.edu.hk/~csjcao/'>Jiannong Cao</a><sup>†</sup>, <a href='https://pr-ryan.github.io/'>Penghui Ruan</a><sup>†‡</sup>, <a href='https://divvyasaxena.github.io/Divya-Saxena/'>Divya Saxena</a><sup>§</sup>, <a href='https://www.zhusongye.com/'>Songye Zhu</a><sup>†</sup>, <a href='https://cyfaaa.github.io/'>Yinfeng Cao</a><sup>†♯</sup>"
affiliations: |
  <sup>†</sup>The Hong Kong Polytechnic University, Hong Kong, China<br>
  <sup>†‡</sup>Southern University of Science and Technology, Shenzhen, China<br>
  <sup>§</sup>Indian Institute of Technology, Jodhpur, India<br>
  <sup>♯</sup>Institute of Cyberspace Technology, HKCT Institute of Higher Education, Hong Kong, China
paper: "https://"
code: "https://github.com/liyw420/GC-4DGS"

---

<div class="columns is-centered has-text-centered">
    <div class="column is-four-fifths">
        <h2>Demo Videos with Sparse Input Views</h2>
        <div class="content has-text-justified">

<!-- 对比容器 -->
<div class="comparison-container fixed-size">
  <img src="/video/gc4dgs_rgb.gif">
  <img src="/video/4dgaussians_rgb.gif">
  <div class="label left">4DGaussians RGB (CVPR'24)</div>
  <div class="label right">GC-4DGS RGB (Ours)</div>
</div>

<div class="comparison-container fixed-size">
  <img src="/video/gc4dgs_depth.gif">
  <img src="/video/4dgaussians_depth.gif">
  <div class="label left">4DGaussians Depth (CVPR'24)</div>
  <div class="label right">GC-4DGS Depth (Ours)</div>
</div>

<div class="comparison-container auto-size">
  <img src="/video/gc4dgs_rgb_add.gif" onload="adjustContainer(this)">
  <img src="/video/4dgs_rgb_add.gif">
  <div class="label left">4DGS RGB (ICLR'24)</div>
  <div class="label right">GC-4DGS RGB (Ours)</div>
</div>

<div class="comparison-container auto-size">
  <img src="/video/gc4dgs_depth_add.gif" onload="adjustContainer(this)">
  <img src="/video/4dgs_depth_add.gif">
  <div class="label left">4DGS Depth (ICLR'24)</div>
  <div class="label right">GC-4DGS Depth (Ours)</div>
</div>

<script>
const FIXED_WIDTH = 800;
const FIXED_HEIGHT = 450;

function adjustContainer(img) {
    const container = img.parentElement;
    if (!img.complete || img.naturalWidth === 0) return;
    
    let width = img.naturalWidth;
    let height = img.naturalHeight;
    
    // 保持原图宽高比，但确保宽度不小于固定尺寸
    if (width < FIXED_WIDTH) {
        const ratio = FIXED_WIDTH / width;
        width = FIXED_WIDTH;
        height = Math.floor(height * ratio);
    }
    
    container.style.width = width + 'px';
    container.style.height = height + 'px';
    
    // 更新滑块高度
    const slider = container.querySelector('.slider');
    if (slider) {
        slider.style.height = container.offsetHeight + 'px';
    }
}

// 统一事件处理
document.querySelectorAll('.comparison-container').forEach(container => {
    const imgs = container.querySelectorAll('img');
    const slider = document.createElement('div');
    slider.className = 'slider';
    container.appendChild(slider);
    
    // 设置第二张图的初始裁剪
    if (imgs[1]) {
        imgs[1].style.clipPath = 'inset(0 50% 0 0)';
    }
    
    // 设置滑块初始高度
    setTimeout(() => {
        slider.style.height = container.offsetHeight + 'px';
    }, 0);
    
    // 直接监听鼠标移动事件 - 无需点击！
    container.addEventListener('mousemove', (e) => {
        handleInteraction(e);
    });
});

function handleInteraction(e) {
    const container = e.currentTarget;
    const rect = container.getBoundingClientRect();
    const x = Math.max(0, Math.min(e.clientX - rect.left, rect.width));
    const slider = container.querySelector('.slider');
    const afterImg = container.querySelectorAll('img')[1];
    
    // 确保滑块始终可见
    slider.style.display = 'flex';
    slider.style.opacity = '1';
    slider.style.background = 'rgba(255,255,255,0.9)';
    slider.style.zIndex = '10';
    
    // 直接更新位置，不使用过渡动画避免延迟
    slider.style.transition = 'none';
    slider.style.left = x + 'px';
    afterImg.style.clipPath = `inset(0 ${rect.width - x}px 0 0)`;
    
    // 在下一帧恢复过渡效果
    requestAnimationFrame(() => {
        slider.style.transition = 'all 0.1s ease';
    });
}
</script>

<style>
.comparison-container {
    position: relative;
    margin: 30px auto;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(0,0,0,0.15);
    cursor: ew-resize;
    user-select: none;
}

.fixed-size {
    width: 800px;
    height: 450px;
}

.auto-size {
    max-width: 800px;
}

.comparison-container img {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    pointer-events: none;
}

.auto-size img {
    object-fit: contain;
}

.label {
    position: absolute;
    bottom: 15px;
    z-index: 5;
    color: white;
    padding: 8px 16px;
    border-radius: 20px;
    font-family: Arial;
    font-size: 14px;
    font-weight: 600;
    backdrop-filter: blur(5px);
}

.label.left {
    left: 15px;
    background: linear-gradient(135deg, rgba(0,0,0,0.7), rgba(0,0,0,0.5));
    border: 1px solid rgba(255,255,255,0.1);
}

.label.right {
    right: 15px;
    background: linear-gradient(135deg, rgba(74,144,226,0.9), rgba(58,123,213,0.8));
    border: 1px solid rgba(255,255,255,0.2);
}

.slider {
    position: absolute;
    top: 0;
    left: 50%;
    width: 4px;
    height: 100%;
    background: rgba(255,255,255,0.9);
    z-index: 10;
    box-shadow: 0 0 10px rgba(0,0,0,0.3);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.1s ease;
}

.slider::before {
    content: '';
    width: 40px;
    height: 40px;
    background: rgba(255,255,255,0.95);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.3);
    border: 2px solid rgba(255,255,255,0.8);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.slider::after {
    content: '';
    width: 6px;
    height: 6px;
    background: rgba(100,100,100,0.6);
    border-radius: 50%;
    position: absolute;
}

.slider:hover::before {
    transform: scale(1.1);
    box-shadow: 0 4px 15px rgba(0,0,0,0.4);
}
</style>

Our Geometry-Consistent 4D Gaussian Splatting (GC-4DGS) achieves high-fidelity rendering quality with only 3 input views. Existing dynamic Gaussian Splatting methods, e.g., 4DGaussians [1], learn incorrect 4D geometry from sparse training views. GC-4DGS solves this issue by learning consistent geometry from both MVS and monocular depths, achieving realistic appearance and coherent geometry of dynamic scenes.

        </div>
    </div>
</div>

---

<div class="columns is-centered has-text-centered">
    <div class="column is-four-fifths">
        <h2>Method Overview</h2>
        <div class="content has-text-justified">

<div style="width: 100%; display: flex; justify-content: center;">
  <img src="/static/image/pipeline.png" alt="Method Overview" style="max-width: 110%; height: auto;">
</div>



(a) We introduce a dynamic consistency checking strategy to fuse view-consistent metric depths from a learning-based MVS method, which is then employed to obtain point clouds for Gaussian initialization and to supervise the learning of 4D geometry. (b) We propose a global-local depth regularization method to distill robust geometry information from a pre-trained MDE, which ensures consistent depth ranking while maintaining local patch smoothness. The optimization is conducted through temporal slicing, differentiable rendering, and color and depth supervision.<br>

        </div>
    </div>
</div>
---

<div class="columns is-centered has-text-centered">
<div class="column is-four-fifths">
<h2>Results</h2>
<div class="content has-text-justified">
<h3 class="title is-4 has-text-weight-bold">Comparisons with 3 input views</h3>

For N3DV Dataset, HyperReel [2] and K-planes [3] fails to render
the scene with few input views. Although Gaussian Splatting-
based approaches improve rendering qualities, they struggle
to capture fine-grained dynamics (e.g., the dog’s hair) and
under-observed geometry (e.g., the mini statue). In contrast,
GC-4DGS recovers more textural and structural details in both
central dynamic areas and under-observed static regions.

For Technicolor Dataset, HyperReel [2] and 4DGaussians [1] produce significantly distorted images, while STG [4] struggles to capture complex dynamics. 4DGS [5] and E-D3DGS [6] are prone to overfit in areas with limited observations and produce blurred results.<br> <br>

<style>
.multi-compare-container {
  position: relative;
  width: 100%;
  max-width: 1000px;
  height: 520px;
  margin: 0 auto 24px auto;
  background: #222;
  border-radius: 8px;
  overflow: hidden;
}
.multi-compare-img {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  object-fit: cover;
  pointer-events: none;
}
.multi-compare-bar {
  position: absolute;
  top: 0; width: 3px; height: 100%;
  background: rgba(255,255,255,0.85);
  z-index: 10;
  cursor: ew-resize;
  transition: background .2s;
}
.multi-compare-bar.active { background: #3aa157; }
.multi-compare-handle {
  position: absolute;
  left: -10px; bottom: 18px;
  width: 20px; height: 20px;
  border-radius: 50%;
  background: rgba(255,255,255,0.88);
  box-shadow: 0 0 0 2px rgba(0,0,0,.15);
}
.multi-compare-label {
  position: absolute;
  bottom: 12px;
  left: 0;
  width: 14.2857%;
  text-align: center;
  background: rgba(0,0,0,0);
  color: #fff;
  font-size: 1rem;
  padding: 4px 0;
  border-radius: 4px;
  pointer-events: none;
  border: none;
  box-shadow: none;
  text-decoration: none;
}
@media (max-width: 768px){
  .multi-compare-container{ height:340px; }
  .multi-compare-label{ font-size:.85rem; }
}
.multi-compare-tabs {
  display: flex;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto .75rem auto;
  gap: 0;
  padding: 0;
}
.multi-compare-tabs button {
  flex: 1 1 0;
  min-width: 0;
  border: 1px solid #dcdcdc;
  background: #f6f6f6;
  border-radius: 0;
  cursor: pointer;
  font-size: 1rem;
  padding: .5rem .9rem;
  transition: background .2s, color .2s;
  border-right: none;
}
.multi-compare-tabs button:first-child {
  border-radius: .5rem 0 0 .5rem;
}
.multi-compare-tabs button:last-child {
  border-radius: 0 .5rem .5rem 0;
  border-right: 1px solid #dcdcdc;
}
.multi-compare-tabs button.active {
  background: #3aa157;
  color: #fff;
  border-color: #3aa157;
  z-index: 1;
}

</style>

<!-- 场景选项卡 -->
<div class="multi-compare-tabs" id="multiCompareTabs">
  <button class="active" data-scene="0">Coffee Martini</button>
  <button data-scene="1">Cut Beef</button>
  <button data-scene="2">Flame Steak</button>
  <button data-scene="3">Birthday</button>
  <button data-scene="4">Train</button>
  <button data-scene="5">Painter</button>
</div>

<div class="multi-compare-container" id="multiCompare"></div>

<script>
// 两组方法标签
const methodNamesA = [
  "HyperReel (CVPR'23)",
  "K-Planes (CVPR'23)",
  "4DGaussians (CVPR'24)",
  "4DGS (ICLR'24)",
  "E-D3DGS (ECCV‘24)",
  "GC-4DGS (Ours)",
  "GT"
];
const methodNamesB = [
  "HyperReel (CVPR'23)",
  "4DGaussians (CVPR'24)",
  "4DGS (ICLR'24)",
  "STG (CVPR'24)",
  "E-D3DGS (ECCV‘24)",
  "GC-4DGS (Ours)",
  "GT"
];

// 场景图片（请替换为你的实际图片路径）
const scenes = [
  // Coffee Martini
  [
    "static/image/figureN3DV/hyper44.png",
    "static/image/figureN3DV/kplanes44.png",
    "static/image/figureN3DV/4dgaussians44.png",
    "static/image/figureN3DV/4dgs44.png",
    "static/image/figureN3DV/ed3dgs44.png",
    "static/image/figureN3DV/gc4dgs44.png",
    "static/image/figureN3DV/gt44.png"
  ],
  // Cut Beef
  [
    "static/image/figureN3DV/hyper2.png",
    "static/image/figureN3DV/kplanes2.png",
    "static/image/figureN3DV/4dgaussians2.png",
    "static/image/figureN3DV/4dgs2.png",
    "static/image/figureN3DV/ed3dgs2.png",
    "static/image/figureN3DV/gc4dgs2.png",
    "static/image/figureN3DV/gt2.png"
  ],
  // Flame Steak
  [
    "static/image/appendix1/hyper.png",
    "static/image/appendix1/kplanes.png",
    "static/image/appendix1/4dgaussians.png",
    "static/image/appendix1/4dgs.png",
    "static/image/appendix1/ed3dgs.png",
    "static/image/appendix1/ours.png",
    "static/image/appendix1/gt.png"
  ],
  // Birthday
  [
    "static/image/figures2/hyper.png",
    "static/image/figures2/4dgaussians.png",
    "static/image/figures2/4dgs.png",
    "static/image/figures2/stg.png",
    "static/image/figures2/ed3dgs.png",
    "static/image/figures2/ours.png",
    "static/image/figures2/gt_frame42.png"
  ],
  // Train
  [
    "static/image/figures2/hyper2.png",
    "static/image/figures2/4dgaussians2.png",
    "static/image/figures2/4dgs2.png",
    "static/image/figures2/stg2.png",
    "static/image/figures2/ed3dgs2.png",
    "static/image/figures2/ours2.png",
    "static/image/figures2/gt_frame2.png"
  ],
  // Painter
  [
    "static/image/appendix2/hyper12.png",
    "static/image/appendix2/4dgaussians12.png",
    "static/image/appendix2/4dgs12.png",
    "static/image/appendix2/stg12.png",
    "static/image/appendix2/ed3dgs12.png",
    "static/image/appendix2/ours12.png",
    "static/image/appendix2/gt12.png"
  ]
];

const N = 7; // 图片数量
const barCount = N - 1; // 分界线数量

function getMethodNames(sceneIdx) {
  // 前3个场景用A，后3个用B
  return sceneIdx <= 2 ? methodNamesA : methodNamesB;
}

function createCompareDOM(imgList, methodNames) {
  // 清空
  const container = document.getElementById('multiCompare');
  container.innerHTML = '';
  // 图片
  for(let i=0;i<N;i++) {
    const img = document.createElement('img');
    img.src = imgList[i];
    img.className = `multi-compare-img img${i}`;
    img.alt = methodNames[i] || '';
    container.appendChild(img);
  }
  // 分界线
  for(let i=0;i<barCount;i++) {
    const bar = document.createElement('div');
    bar.className = 'multi-compare-bar';
    bar.setAttribute('data-i', i);
    bar.innerHTML = '<span class="multi-compare-handle"></span>';
    bar.style.left = ((i+1)*100/N) + '%';
    container.appendChild(bar);
  }
  // 标签
  for(let i=0;i<N;i++) {
    const label = document.createElement('div');
    label.className = 'multi-compare-label';
    label.style.left = (i*100/N) + '%';
    label.style.width = (100/N) + '%';
    label.textContent = methodNames[i] || '';
    container.appendChild(label);
  }
}

function initCompare(imgList, methodNames) {
  createCompareDOM(imgList, methodNames);

  // 初始分界线位置（均分）
  let bars = [];
  for(let i=0;i<barCount;i++) bars[i] = (i+1)*100/N;

  const container = document.getElementById('multiCompare');
  const imgEls = [];
  for(let i=0;i<N;i++) imgEls[i] = container.querySelector(`.img${i}`);
  const barEls = container.querySelectorAll('.multi-compare-bar');
  const labelEls = container.querySelectorAll('.multi-compare-label');
  let dragging = null;
  let dragStartX = 0;
  let dragStartBars = [];

  function updateImages(){
    // 裁剪区域
    imgEls[0].style.clipPath = `inset(0 ${100-bars[0]}% 0 0)`;
    for(let i=1;i<N-1;i++){
      imgEls[i].style.clipPath = `inset(0 ${100-bars[i]}% 0 ${bars[i-1]}%)`;
    }
    imgEls[N-1].style.clipPath = `inset(0 0 0 ${bars[N-2]}%)`;
    // 分界线
    barEls.forEach((bar,i)=>{ bar.style.left = bars[i] + '%'; });
    // 标签
    labelEls[0].style.left = '0%';
    labelEls[0].style.width = bars[0] + '%';
    for(let i=1;i<N;i++){
      labelEls[i].style.left = bars[i-1] + '%';
      labelEls[i].style.width = ((bars[i]||100) - bars[i-1]) + '%';
    }
  }

  // 拖动分界线
  barEls.forEach((bar,i)=>{
    bar.addEventListener('mousedown', function(e){
      dragging = i;
      dragStartX = e.clientX;
      dragStartBars = bars.slice();
      bar.classList.add('active');
      document.body.style.userSelect = 'none';
    });
    bar.addEventListener('touchstart', function(e){
      dragging = i;
      dragStartX = e.touches[0].clientX;
      dragStartBars = bars.slice();
      bar.classList.add('active');
    }, {passive:true});
  });

  function moveBar(clientX){
    if(dragging===null) return;
    const rect = container.getBoundingClientRect();
    let dx = clientX - dragStartX;
    let percentDx = dx / rect.width * 100;
    let newBars = dragStartBars.slice();

    // 计算本条线的新位置
    let newPos = dragStartBars[dragging] + percentDx;

    // 推带逻辑
    if(newPos > dragStartBars[dragging]) {
      newBars[dragging] = newPos;
      for(let j=dragging+1;j<barCount;j++){
        if(newBars[j]<newBars[j-1]) newBars[j]=newBars[j-1];
      }
      // 限制最右端
      for(let j=dragging;j<barCount;j++){
        if(j===barCount-1 && newBars[j]>100) newBars[j]=100;
        else if(j<barCount-1 && newBars[j]>newBars[j+1]) newBars[j]=newBars[j+1];
      }
    } else {
      newBars[dragging] = newPos;
      for(let j=dragging-1;j>=0;j--){
        if(newBars[j]>newBars[j+1]) newBars[j]=newBars[j+1];
      }
      // 限制最左端
      for(let j=dragging;j>=0;j--){
        if(j===0 && newBars[j]<0) newBars[j]=0;
        else if(j>0 && newBars[j]<newBars[j-1]) newBars[j]=newBars[j-1];
      }
    }
    bars = newBars;
    updateImages();
  }

  document.addEventListener('mousemove', e=>{ if(dragging!==null) moveBar(e.clientX); });
  document.addEventListener('touchmove', e=>{
    if(dragging!==null && e.touches[0]) moveBar(e.touches[0].clientX);
  }, {passive:false});
  document.addEventListener('mouseup', ()=>{
    if(dragging!==null) barEls[dragging].classList.remove('active');
    dragging=null; document.body.style.userSelect='';
  });
  document.addEventListener('touchend', ()=>{
    if(dragging!==null) barEls[dragging].classList.remove('active');
    dragging=null;
  });

  updateImages();
}

// 场景切换
const tabs = document.getElementById('multiCompareTabs');
tabs.addEventListener('click', function(e){
  const btn = e.target.closest('button[data-scene]');
  if(!btn) return;
  tabs.querySelectorAll('button').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  const idx = parseInt(btn.getAttribute('data-scene'),10);
  const methodNames = getMethodNames(idx);
  initCompare(scenes[idx], methodNames);
});

// 初始化默认场景
initCompare(scenes[0], getMethodNames(0));
</script>


<h3 class="title is-4 has-text-weight-bold">Ablation Studies</h3>

MVS contributes to the learning of fine-grained 4D geometry with dynamic consistency checking (DCC). Furthermore, the proposed depth regularization components provide complementary benefits that enhance geometric consistency. <br>
<br>

 <img src="/static/image/figure 5.jpg" alt="Ablation Studies" style="max-width: 100%; height: auto;">

<h3 class="title is-4 has-text-weight-bold">Deployability on Edge Devices</h3>

To further evaluate the deployability of GC-4DGS on resource-constrained edge devices within IoT systems, experiments are conducted on the Jetson AGX Orin Developer Kit. Our model was first trained in the cloud before being deployed on edge devices for testing. The results demonstrate that GC-4DGS is easily deployable on IoT edge devices with limited computing resource consumption, highlighting the potential of our model for AIoT applications. <br>
<br>

 <img src="/static/image/figure 6.jpg" alt="Deployability" style="max-width: 100%; height: auto;">

</div>
</div>
</div>

---

<div class="columns is-centered has-text-centered">
    <div class="column is-four-fifths">
        <h2>References</h2>
        <div class="content has-text-justified">

[1] G. Wu, T. Yi, J. Fang, L. Xie, X. Zhang, W. Wei, W. Liu, Q. Tian, and X. Wang, “4d gaussian splatting for real-time dynamic scene rendering,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2024, pp. 20 310–20 320.<br>
[2] B. Attal, J.-B. Huang, C. Richardt, M. Zollhoefer, J. Kopf, M. O’Toole, and C. Kim, “Hyperreel: High-fidelity 6-dof video with ray-conditioned sampling,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 16 610–16 620.<br>
[3] S. Fridovich-Keil, G. Meanti, F. R. Warburg, B. Recht, and A. Kanazawa, “K-planes: Explicit radiance fields in space, time, and appearance,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2023, pp. 12 479–12 488.<br>
[4] Z. Li, Z. Chen, Z. Li, and Y. Xu, “Spacetime gaussian feature splatting for real-time dynamic view synthesis,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2024, pp.8508–8520.<br>
[5] Z. Yang, H. Yang, Z. Pan, and L. Zhang, “Real-time photorealistic dynamic scene representation and rendering with 4d gaussian splatting,” in ICLR, 2024.<br>
[6] J. Bae, S. Kim, Y. Yun, H. Lee, G. Bang, and Y. Uh, “Per-gaussianembedding-based deformation for deformable 3d gaussian splatting,” in European Conference on Computer Vision. Springer, 2025, pp. 321–335.<br>

</div>
</div>
</div>

---
