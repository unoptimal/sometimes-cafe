<script>
    import { db } from '../backend/firebase.js';
    import { onValue, ref, set } from 'firebase/database';
    import { Duration } from 'luxon';
    import Stats from './Stats.svelte';
    
const statusRef = ref(db, 'status');   
let displayMessage = 'OPEN';
let timesOpened;
let isOpen = false;


let totalOpenDuration = Duration.fromMillis(0);
let totalClosedDuration = Duration.fromMillis(0);
let currentOpenDuration = Duration.fromMillis(0)
let currentClosedDuration = Duration.fromMillis(0)

let length = Math.floor(Math.random() * (1800 - 600 + 1)) + 600; 
let minutes = Math.floor(length / 60); 
let seconds = length % 60; 
let formattedLength = `${minutes} minutes and ${seconds} seconds`; 

let intervalId = null;

let openingClosingTimestamps = {};

onValue(statusRef, (snapshot) => {
    const data = snapshot.val();
    timesOpened = !isNaN(data.timesOpened) ? data.timesOpened : 0;
    
//     // totalOpenDuration = Duration.fromMillis(data.totalOpenDuration * 1000 || totalOpenDuration.toMillis());
//     // totalClosedDuration = Duration.fromMillis(data.totalClosedDuration * 1000 || totalClosedDuration.toMillis());
//     // currentOpenDuration = Duration.fromMillis(data.currentOpenDuration || 0);
//     // currentClosedDuration = Duration.fromMillis(data.currentClosedDuration || 0);
//     // openingClosingTimestamps = data.openingClosingTimestamps || {};
});

// function switchStatus(){
//     isOpen = !isOpen;
//     let length = Math.floor(Math.random() * 5) + 2;
//     let minutes = Math.floor(length / 60); 
//     let seconds = length % 60; 
//     formattedLength = `${minutes} minutes and ${seconds} seconds`; 

// }

function updateStatus(){
    isOpen = !isOpen;
    displayMessage = isOpen ? 'OPEN' : 'CLOSED';
    const now = new Date();
    timesOpened = timesOpened + (isOpen ? 1 : 0);

    if (isOpen) {
      currentClosedDuration = Duration.fromMillis(0);
      if (!intervalId) {
        intervalId = setInterval(() => {
          totalOpenDuration = totalOpenDuration.plus({ seconds: 1 });
          currentOpenDuration = currentOpenDuration.plus({ seconds: 1 });
          if (currentOpenDuration.as('seconds') >= length) {
            clearInterval(intervalId);
            intervalId = null;
            set(statusRef, { timesOpened
            //     totalOpenDuration: totalOpenDuration.as('seconds') 
            });
            
            updateStatus();
          }
        }, 1000);
      }
    //   openingClosingTimestamps[timesOpened] = { start: now.toISOString() };
    } else {
      currentOpenDuration = Duration.fromMillis(0);
      if (!intervalId) {
        intervalId = setInterval(() => {
          totalClosedDuration = totalClosedDuration.plus({ seconds: 1 });
          currentClosedDuration = currentClosedDuration.plus({ seconds: 1 });
          if (currentClosedDuration.as('seconds') >= length) {
            clearInterval(intervalId);
            intervalId = null;
            set(statusRef, { timesOpened
            //     totalClosedDuration: totalClosedDuration.as('seconds') 
            });
            updateStatus();
          }
        }, 1000);
      }
    //   openingClosingTimestamps[timesOpened].end = now.toISOString();
    }
    
  }

  updateStatus();

</script>

<p>The Sometimes Cafe is now: <b>{displayMessage}</b>.</p>
<p>It has been opened a total of {timesOpened} times.</p> 
<p><i>(Currently, the database is correctly storing the total open count on a randomly set timer [between 10-30 min].)</i></p>



<Stats
    currentOpenDuration={currentOpenDuration}
    currentClosedDuration={currentClosedDuration}
    totalOpenDuration={totalOpenDuration}
    totalClosedDuration={totalClosedDuration} 
    formattedLength={formattedLength}
/>


<!-- 
openingClosingTimestamps={openingClosingTimestamps}
-->

