<script>
  import { onDestroy } from 'svelte';
  import ScrollyVideo from './ScrollyVideo.js';

  // variable to hold the DOM element
  export let scrollyVideoContainer = null;

  // variable to hold the scrollyVideo object
  export let scrollyVideo = null;

  // Store the props so we know when things change
  let lastPropsString = '';

  $: {
    if (scrollyVideoContainer) {
      // separate out the videoPercentage prop
      const { videoPercentage, ...restProps } = $$props;

      if (JSON.stringify(restProps) !== lastPropsString) {
        // if scrollyvideo already exists and any parameter is updated, destroy and recreate.
        if (scrollyVideo && scrollyVideo.destroy) scrollyVideo.destroy();
        scrollyVideo = new ScrollyVideo({ ...restProps, scrollyVideoContainer });

        // Save the new props
        lastPropsString = JSON.stringify(restProps);
      }

      // If we need to update the target time percent
      if (scrollyVideo && typeof videoPercentage === 'number' && videoPercentage >= 0 && videoPercentage <= 1) {
        scrollyVideo.setTargetTimePercent(videoPercentage);
      }
    }
  }

  // Cleanup the component on destroy
  onDestroy(() => {
    if (scrollyVideo && scrollyVideo.destroy) scrollyVideo.destroy();
  });
</script>

<div bind:this={scrollyVideoContainer} />
