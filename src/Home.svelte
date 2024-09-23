<style>
    *{
        cursor:none;
    }
    @font-face {
        font-family: 'Futura';
        src: url('/FuturaLT-Bold.woff') format('woff');
        font-weight: bold;
        font-style: normal;
    }

    @font-face {
        font-family: 'Futurabk';
        src: url('/FuturaLT-Book.ttf') format('truetype');
        font-style: normal;
    }

    @font-face {
        font-family: 'Libre';
        src: url('/LibreBodoni-Bold.ttf') format('truetype');
        font-weight: bold;
        font-style: normal;
    }

    @font-face {
        font-family: 'MBFSpace';
        src: url('/MBFSpace.ttf') format('truetype');
        font-weight: bold;
        font-style: normal;
    }

    :global(body) {
        background-image: url('/bg.jpg');
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        height: 100vh;
        margin: 0;
        display: flex;
        flex-direction: column;
        padding: 0;
        background-attachment: fixed;
        font-family: 'MBFSpace';
    }

    .logo-container {
        display: flex;
        flex-direction: column;
        justify-content: center; /* Center vertically */
        align-items: center; /* Center horizontally */
        text-align: center;
        height: 100%; /* Full height of the body */
        width: 100%; /* Full width of the body */
        box-sizing: border-box;
        padding: 20px; /* Padding to avoid edge collision */
    }

    .logo {
        width: 100%; /* Full width with a maximum width */
        max-width: 450px; /* Maximum width to ensure it doesn't grow too large */
        height: auto; /* Maintain aspect ratio */
        margin-bottom: 15px; /* Adjusted margin */
    }

    .title {
        font-size: 60px; /* Responsive font size */
        text-align: center;
        color: white; /* Set text color to white */
        word-spacing: 2vw; /* Responsive word spacing */
        margin: 15px 0; /* Adjusted margin */
    }

    @media (max-width: 768px) {
        .logo {
            max-width: 80vw; /* Smaller max-width for phones */
        }

        .title {
            font-size: 10vw; /* Larger font size for better readability on small screens */
            word-spacing: 4vw; /* Larger word spacing for readability on small screens */
        }

        .logo-container {
            padding: 30px; /* Increase padding to provide more space around the content */
        }
    }

    @media (max-width: 480px) {
        .logo {
            max-width: 60vw; /* Further reduce max-width for very small screens */
        }

        .title {
            font-size: 10vw; /* Larger font size for very small screens */
            word-spacing: 6vw; /* Larger word spacing for very small screens */
        }

        .logo-container {
            padding: 40px; /* Increase padding further for very small screens to ensure centering */
        }
    }
</style>

<div class="logo-container">
    <img src="/cosmologo.png" alt="Logo" class="logo">
    <div class="title">INFINITY BECKONS US</div>
</div>
