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
    background-color: rgba(51, 51, 51, 0);
    color: white;
    padding: 10px 20px;
    position: relative;
    font-family: 'MBFSpace', serif;
    font-weight: bold;
    font-size: 24px;
    z-index: 1000;
  }

  .burger-menu {
    display: none;
    cursor: pointer;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 50px;
    height: 35px;
    position: fixed;
    right: 15px;
    top: 10px;
    z-index: 1200;
  }

  .burger-menu div {
    width: 100%;
    height: 6px;
    background-color: white;
    margin: 3px 0;
    transition: transform 0.3s ease, opacity 0.3s ease;
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
  }

  .menu-items a:hover {
    transition: transform 0.3s;
    transform: scale(1.1);
    background-color: rgba(85, 85, 85, 0.74);
    border-radius: 8px;
  }

  .dropdown:hover .dropbtn {
    background-color: rgba(85, 85, 85, 0.74);
    border-radius: 8px;
  }

  .dropdown {
    position: relative;
    display: inline-block;
    text-align: center;
  }

  .dropbtn {
    display: flex;
    align-items: center;
  }

  .dropdown-content {
    display: none;
    position: absolute;
    background-color: #222;
    min-width: 200px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    z-index: 1;
    margin-top: 0;
    font-size: 14px;
    border-radius: 8px;
    word-spacing: 2px;
  }

  .dropdown-content a {
    color: white;
    padding: 10px 12px;
    text-decoration: none;
    display: block;
    text-align: center;
    position: relative;
  }

  .dropdown-content a:hover {
    background-color: rgba(70, 70, 70, 0);
  }

  .dropdown-content.show {
    display: block;
  }

  @media screen and (max-width: 768px) {
    .burger-menu {
      display: block;
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

    .burger-menu div {
      width: 40px;
      height: 5px;
      background-color: white;
      margin: 5px auto;
      transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
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

    .dropdown {
      display: flex;
      flex-direction: column;
      justify-content: center;
      margin-right: 8px;
      align-self: center;
    }

    .menu-items a:hover {
      transform: scale(1.1);
      background-color: rgba(85, 85, 85, 0.27);
      border-radius: 8px;
    }

    .dropdown-content {
      display: none;
      position: relative;
      background-color: #222;
      min-width: 100%;
      box-shadow: none;
      z-index: 1;
      margin-top: 15px;
      font-size: 18px;
      right: 0;
      left: 50%;
      transform: translateX(-50%);
    }

    .dropdown-content a {
      text-align: center;
      font-size: 28px;
    }

    .dropdown-content.show {
      display: block;
    }
  }

  @keyframes fall {
    0% {
      transform: translateY(-100%);
      opacity: 0;
    }
    100% {
      transform: translateY(0);
      opacity: 1;
    }
  }
</style>

<div class="navbar">
  <div class="menu-items {showMenu ? 'show' : ''}">
    <a href="/" on:click={handleClick}>Home</a>
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
