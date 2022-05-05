<script>
    import { onMount } from "svelte";
    let msgs = [];
    var usrMsg = "";
    let socket;
    var ready = false;

    var randomUsername = "_" + Math.random().toString(36).substring(2, 9);

    onMount(() => {
        msgs = msgs;
        socket = new WebSocket("wss://1825-150-107-106-30.in.ngrok.io");
        socket.addEventListener("open", () => {
            console.log("Opened");
        });
        socket.onmessage = function (e) {
            var server_message = e.data;
            msgs.push(server_message);
            msgs = msgs;
        };

        socket.onopen = function () {
            ready = !ready;
            socket.send(randomUsername+" joined the chat room!")
        };

        // sendMsg() = function {
        //     socket.send(usrMsg)
        // }
    });

    const sendMessage = () => {
        socket.send(randomUsername + " ://> " + usrMsg);
        usrMsg = "";
    };
</script>

<div class="container">
    <div class="header">You are {randomUsername} hai ta</div>

    <div class="msg-container">
        {#each msgs as msg}
         {#if msg.includes(randomUsername)}
            <div class="message sent">
               {msg}
           </div> 
        {:else}
        <div class="message">
            {msg}
        </div> 
      
         {/if}    

            
        {/each}
    </div>

    {#if ready}
     
        <input
            type="text"
            class="msgBox"
            bind:value={usrMsg}
            placeholder="Type your msg here..."
        />
        <button class="submit" on:click={sendMessage}>Send</button>
    {/if}
</div>

<style>
 
    .msgBox {
        width: 60vw;
        margin: 10px;
        border: none;
        padding: 7px;
        font-weight: bold;
        position: absolute;
        bottom: 0;

        margin-bottom: 20px;
        background-color: burlywood;
    }

    .submit{
        width: 30vw;
        background-color: blue;
        border: none;
        margin: 10px;
        margin-bottom: 20px;
        padding: 7px;
        color: white;
        font-weight: bold;
        position: absolute;
        bottom: 0;
        right: 0;
    }

    .header {
        text-align: center;
        font-weight: bold;
        padding: 10px;
    }
    .container {
        background-color: aliceblue;
        height: 100vh;
        width: 100vw;
    }

    .msg-container {
        padding: 10px;
    }

    .message {
        background-color: white;
        color: black;
        font-weight: bold;
        padding-left: 15px;
        padding-right: 15px;
        padding-top: 5px;
        padding-bottom: 5px;
        margin: 15px;
        border-radius: 25px;
    }

    .sent {
        background-color: purple;
        color: white;
        font-weight: bold;
    }
</style>
