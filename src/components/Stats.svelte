<script>
//   import { DateTime } from 'luxon';

  //   export let isOpen;

  
//   export let openingClosingTimestamps;

//   function formatTimestamp(timestamp) {
//     return DateTime.fromISO(timestamp).toLocaleString(DateTime.DATETIME_SHORT_WITH_SECONDS);
// }
    import { CollapsibleCard } from 'svelte-collapsible'
    const duration = 0.2 // seconds
    const easing = 'linear'
    export let currentOpenDuration;
    export let currentClosedDuration;
    export let totalOpenDuration;
    export let totalClosedDuration;
    export let formattedLength
</script>

<CollapsibleCard { duration } { easing }>

    <h2 slot='header'>More Stats</h2>
    <div slot='body'>
        <p><i>Note: none of these values are properly stored server-side. They are just here for display; refreshing the page sets them to 0.</i></p>
        <ul>
          <li>The next switch will occur in {formattedLength}. 
            <ul>
              <li><small><i>Note: this value is almost certainly incorrect.</i></small></li>
            </ul>
          </li>
          <li>Current open duration: {currentOpenDuration.toFormat('hh:mm:ss')}</li>
          <li>Total open duration: {totalOpenDuration.toFormat('hh:mm:ss')}</li>
          <li>Current closed duration: {currentClosedDuration.toFormat('hh:mm:ss')}</li>
          <li>Total closed duration: {totalClosedDuration.toFormat('hh:mm:ss')}</li>
          <li><i>Opening and closing timestamps: [WIP]</i></li>
        </ul>
    </div>
</CollapsibleCard>

  <style>

:global(.card) {
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.1);
    margin-bottom: 30px;
  }

  li{
    margin-top: 5px;
    margin-bottom: 10px;
  }
  </style>