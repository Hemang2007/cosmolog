<script>
  let items = [
    { img: '/posters/poster1.jpg', pdf: '/pdfs/research1.pdf', title: 'Research 1', author: 'Author 1' },
    { img: '/posters/poster2.jpg', pdf: '/pdfs/research2.pdf', title: 'Research 2', author: 'Author 2' },
    // Add more posters as needed
  ];

  // Function to open the PDF in a new tab
  function openPDF(pdfUrl) {
    window.open(pdfUrl, '_blank');
  }
</script>

<!-- Posters Gallery -->
<div class="gallery">
  {#each items as item}
    <div class="item" on:click={() => openPDF(item.pdf)}>
      <img src={item.img} alt="research" />
      <div class="overlay">
        <div class="text">
          <h3>{item.title}</h3>
          <p>{item.author}</p>
        </div>
      </div>
    </div>
  {/each}
</div>

<style>
  .gallery {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start; /* Left align */
    gap: 20px; /* Space between posters */
    padding: 20px;
    width: 100%;
    margin: 0 auto;
    box-sizing: border-box;
    overflow-y: auto;
    max-height: 70vw;
  }

  .item {
    position: relative;
    width: 220px; /* Fixed width for the poster */
    height: 320px; /* Height for a rectangular shape */
    border-radius: 10px;
    transition: transform 0.3s;
  }

  .item:hover {
    transform: scale(1.05);
  }

  .item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  /* Overlay with animation from bottom to top */
  .overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.7);
    height: 100%;
    opacity: 0;
    transition: all 0.3s ease-in-out;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .item:hover .overlay {
    opacity: 1;
  }

  .text {
    color: white;
    text-align: center;
  }

  .text h3 {
    margin: 0;
    font-size: 16px;
  }

  .text p {
    margin: 0;
    font-size: 14px;
  }

  /* Responsive adjustments */
  @media (max-width: 1200px) {
    .item {
      width: calc(20% - 20px); /* 5 posters per row */
    }
  }

  @media (max-width: 768px) {
    .item {
      width: calc(50% - 20px); /* 2 posters per row on small screens */
    }
  }

  @media (max-width: 480px) {
    .item {
      width: 100%; /* 1 poster per row on very small screens */
    }
  }
</style>