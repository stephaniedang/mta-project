<script>
  import { onMount } from 'svelte';

  let isMobile = false;

  onMount(() => {
    isMobile = window.innerWidth <= 768;
  });
</script>

{#if isMobile}
<div id="mobile-warning">
  <div id="content">
    <img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ2VqMGs5bTVzY3I5d244bDd3ZHdjOGlob2s5bzV5dHo4eTNkY2R1YyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oEdv9pgtUVVYdpaY8/giphy.webp" alt="Funny subway GIF" />
    <h1>Oops! 🎨🚇</h1>
    <p>Looks like you're on a mobile device. My project is a little shy and prefers the big screen!</p>
    <p>For the best experience, hop on a laptop or desktop.</p>
    <p>In the meantime, here’s a fun fact: Did you know the NYC subway system has over 400 stations? That’s a lot of stops for amazing art!</p>
    <p class="sign-off">Appreciate you taking the time!</p>
    <p class="sign-off">- Stephanie</p>
  </div>
</div>
{/if}

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
  @import url('https://fonts.googleapis.com/css2?family=Delius&family=Delius+Unicase:wght@400;700&family=Grandstander:wght@500&family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap');

  #mobile-warning {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #2a2a2a;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    z-index: 9999;
    font-family: 'Poppins', sans-serif;
  }

  #content {
    max-width: 600px;
    padding: 20px;
  }

  img {
    max-width: 100%;
    height: auto;
    margin: 15px 0;
    border-radius: 10px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.3);
  }

  h1 {
    font-size: 2.5em;
    font-family: 'Delius Unicase', cursive;
    margin-bottom: 10px;
    color: #a4dded;
  }

  p {
    margin: 25px;
    font-size: 0.9em;
  }

  .sign-off {
    font-size: 1.2em;
    font-family: 'Delius Unicase', cursive;
    color: #a4dded;
    margin: 5px;
  }
</style>