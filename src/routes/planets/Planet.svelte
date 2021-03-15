<script>
  import { onMount } from "svelte";
  import Meanderer from "meanderer";
  export let handleMouseDown;
  export let size;
  export let name;

  onMount(async () => {
    const CONTAINER = document.getElementById("planet");

    console.log("CONTAINER", CONTAINER);
    // Our path string
    const PATH =
      "M645.773 223.958C697.174 347.2 615.507 498.73 463.071 562.306c-152.435 63.577-317.569 14.98-368.9694-108.261C42.7011 330.803 124.368 179.273 276.804 115.697c152.435-63.5765 317.569-14.98 368.969 108.261z";
    // The bounds of our path
    const WIDTH = 198.73;
    const HEIGHT = 184.9;
    console.log("doc", PATH);
    // Generate a responsive path
    const responsivePath = new Meanderer({
      path: PATH,
      width: WIDTH,
      height: HEIGHT,
    });

    console.log("responsivePath", responsivePath);
    // Generate a new scaled path when required. Here we are using ResizeObserver
    // with a container that uses viewport units
    const setPath = () => {
      const scaledPath = responsivePath.generatePath(
        CONTAINER.offsetWidth,
        CONTAINER.offsetHeight
      );
      // Here, we apply the path to an element through a CSS variable.
      // And then an element picks up on that. We could apply the motion path straight to the element though.
      CONTAINER.style.setProperty("--path", `"${scaledPath}"`);
    };
    // Set up our Resize Observer that will get the ball rolling
    const SizeObserver = new ResizeObserver(setPath);
    // Observe! Done!
    SizeObserver.observe(CONTAINER);
  });
</script>

<div class="planet {name}" on:mousedown={handleMouseDown} id="planet">
  <div class="planet" />
</div>

<style>
  @keyframes move {
    100% {
      motion-offset: 100%;
      offset-distance: 100%;
    }
  }
  .planet {
    border-radius: 50%;
    height: 100px;
    width: 100px;
    position: absolute;
  }
  /* size of .earth is based on sun measuring 'scale(1)' */
  .earth {
    background: rgb(97, 149, 197);
    transform: scale(0.09);

    motion-path: path(
      "M645.773 223.958C697.174 347.2 615.507 498.73 463.071 562.306C310.636 625.883 145.502 577.286 94.1016 454.045C42.7011 330.803 124.368 179.273 276.804 115.697C429.239 52.1205 594.373 100.717 645.773 223.958Z"
    );
    offset-path: path(
      "M645.773 223.958C697.174 347.2 615.507 498.73 463.071 562.306C310.636 625.883 145.502 577.286 94.1016 454.045C42.7011 330.803 124.368 179.273 276.804 115.697C429.239 52.1205 594.373 100.717 645.773 223.958Z"
    );

    animation: move 36.5s linear infinite;
  }
</style>
