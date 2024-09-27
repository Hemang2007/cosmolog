  <script>
    import { navigate } from 'svelte-routing';
    import { onMount } from 'svelte';

    let showMenu = false;
    let dropdowns = {
      '#about': false,
      '#committees': false
    };

    function handleClick(event) {
      const href = event.currentTarget.getAttribute('href');
      const isDropdown = event.currentTarget.classList.contains('dropbtn');

      if (!isDropdown) {
        event.preventDefault();
        dropdowns = {
          '#about': false,
          '#committees': false
        };

        if (href.startsWith('http')) {
          window.open(href, '_blank');
        } else {
          navigate(href);
          if (window.innerWidth <= 768) {
            showMenu = false;
          }
        }
      } else {
        event.preventDefault();
        const dropdownId = event.currentTarget.getAttribute('href');
        dropdowns = {
          '#about': false,
          '#committees': false,
          [dropdownId]: !dropdowns[dropdownId]
        };
      }
    }

    function toggleMenu() {
      showMenu = !showMenu;
      if (!showMenu) {
        dropdowns = {
          '#about': false,
          '#committees': false
        };
      }
    }

    function handleKeydown(event) {
      if (event.key === 'Enter' || event.key === ' ') {
        event.preventDefault();
        toggleMenu();
      }
    }

    function handleClickOutside(event) {
      if (showMenu && !event.target.closest('.navbar')) {
        showMenu = false;
      }
      if (!event.target.closest('.dropdown')) {
        dropdowns = {
          '#about': false,
          '#committees': false
        };
      }
    }

    onMount(() => {
      document.addEventListener('click', handleClickOutside);

      return () => {
        document.removeEventListener('click', handleClickOutside);
      };
    });
  </script>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      cursor: none;
    }

    .navbar {
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: rgb(0, 0, 0); /* Keep navbar black */
      color: white;
      padding: 10px 20px;
      position: relative;
      font-family: 'MBFSpace', serif;
      font-weight: bold;
      font-size: 24px;
      z-index: 1000;
      max-width: 100vw;
      overflow-x: hidden;
    }

    /* Logo on the left */
    .logo {
      margin-right: auto; /* Align logo to the left */
    }

    .logo-image {
      height: 50px; /* Adjust the size of the logo */
      width: auto; /* Maintain aspect ratio */
    }

    .menu-items {
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .menu-items a {
      color: white;
      padding: 10px 15px;
      text-decoration: none;
      position: relative;
      overflow: hidden; /* Required for underline animation */
      transition: color 0.3s ease;
    }

    /* Neon underline animation on hover */
    .menu-items a::before {
      content: '';
      position: absolute;
      bottom: 0;
      left: 50%;
      width: 0;
      height: 2px;
      background: rgba(0, 255, 255, 0.8);
      transition: width 0.4s ease, left 0.4s ease;
    }

    .menu-items a:hover::before {
      width: 100%;
      left: 0;
    }

    /* Neon color change on hover */
    .menu-items a:hover {
      color: rgba(0, 255, 255, 0.8);
    }

    .menu-items a:hover {
      animation: neon-glow 1s ease-in-out 0.1s infinite alternate;
    }

    .logo:hover .logo-image {
    filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.8))
            drop-shadow(0 0 10px rgba(255, 255, 255, 0.6))
            drop-shadow(0 0 20px rgb(255, 255, 255))
            drop-shadow(0 0 40px rgba(255, 255, 255, 0.8));
    animation: neon-glow-logo 1.5s ease-in-out infinite alternate;
  }

    @keyframes neon-glow {
      0% {
        text-shadow: 0 0 5px rgba(0, 255, 255, 0.8), 0 0 10px rgba(0, 255, 255, 0.6);
      }
      100% {
        text-shadow: 0 0 20px rgba(0, 255, 255, 1), 0 0 40px rgba(0, 255, 255, 0.8);
      }
    }

    @keyframes neon-glow-logo {
    0% {
      filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.8))
              drop-shadow(0 0 10px rgba(255, 255, 255, 0.6));
    }
    100% {
      filter: drop-shadow(0 0 20px rgb(255, 255, 255))
              drop-shadow(0 0 40px rgba(255, 255, 255, 0.8));
    }
  }

    /* Dropdown hover animation */
    .dropdown:hover .dropbtn {
      background-color: rgba(85, 85, 85, 0.74);
      border-radius: 8px;
    }

    /* Neon glow for dropdown links */
    .dropdown-content a:hover {
      background-color: rgba(85, 85, 85, 0.8);
      color: rgba(0, 255, 255, 0.8);
    }

    /* Burger menu styling */
    .burger-menu {
      display: none;
      cursor: pointer;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 30px;
      height: 25px;
      position: fixed;
      right: 15px;
      top: 10px;
      z-index: 1200;
    }

    .burger-menu div {
      width: 30px;
      height: 4px;
      background-color: white;
      margin: 6px auto;
      transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
    }

    .burger-menu.active div:nth-child(1) {
      transform: rotate(-45deg) translate(-1vw, 1vw);
    }

    .burger-menu.active div:nth-child(2) {
      opacity: 0;
    }

    .burger-menu.active div:nth-child(3) {
      transform: rotate(45deg) translate(-1vw, -1vw);
    }

    /* Responsive Styles */
    @media screen and (max-width: 768px) {
      .burger-menu {
        display: block; /* Burger menu only visible on smaller screens */
      }

      .menu-items {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background-color: rgba(0, 0, 0, 0.8);
        z-index: 1100;
        opacity: 0;
        pointer-events: none;
        transition: opacity 0.1s ease, transform 0.1s ease;
        gap: 10px;
        padding-top: 50px;
        padding-bottom: 40px;
      }

      .menu-items.show {
        display: flex;
        opacity: 1;
        pointer-events: auto;
        flex-direction: column;
        animation: fall 0.3s ease-out forwards;
      }

      .menu-items a {
        padding: 50px 20px;
        text-decoration: none;
        color: white;
        display: block;
        transition: background-color 0.3s ease;
        font-size: 24px;
      }

      .menu-items a:hover {
        background-color: #555;
      }

      .burger-menu.active div:nth-child(1) {
        transform: rotate(-45deg) translate(-8px, 8px);
      }

      .burger-menu.active div:nth-child(2) {
        opacity: 0;
      }

      .burger-menu.active div:nth-child(3) {
        transform: rotate(45deg) translate(-8px, -8px);
      }
    }
  </style>

  <div class="navbar">
    <a href="/" on:click={handleClick} class="logo">
      <img src="cosmologsymbol.png" alt="Logo" class="logo-image" />
    </a>
    <div class="menu-items {showMenu ? 'show' : ''}">
      <a href="/" on:click={handleClick}>Home</a>
      <a href="/about" on:click={handleClick}>About</a>
      <a href="/research" on:click={handleClick}>Research</a>
      <a href="/blog" on:click={handleClick}>Blog</a>
      <a href="/tracker" on:click={handleClick}>3D Tracker</a>
      <a href="/campaign" on:click={handleClick}>Campaign</a>
    </div>
    <div
      class="burger-menu {showMenu ? 'active' : ''}"
      tabindex="0"
      role="button"
      aria-label="Toggle menu"
      on:click={toggleMenu}
      on:keydown={handleKeydown}
    >
      <div></div>
      <div></div>
      <div></div>
    </div>
  </div>
