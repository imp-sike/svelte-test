<script>
    import { onMount } from "svelte";
    let msgs = [];
    var usrMsg = "";
    let socket;
    var ready = false;

    var randomUsername = "_" + Math.random().toString(36).substring(2, 9);

    onMount(() => {


        msgs = msgs;
        socket = new WebSocket("wss://ws-server-1001.herokuapp.com/");
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
            socket.send(randomUsername + " joined the chat room!");
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

<section class="chatbox">
    <section class="chat-window">

        {#each msgs as msg}
         {#if msg.includes(randomUsername)}
         <article class="msg-container msg-self" id="msg-0">
            <div class="msg-box">
                <div class="flr">
                    <div class="messages">
                        <p class="msg" id="msg-0">
                            {msg}
                        </p>
                    </div>
                </div>
            </div>
        </article>
        {:else}
        <article class="msg-container msg-remote" id="msg-0">
            <div class="msg-box">
                <div class="flr">
                    <div class="messages">
                        <p class="msg" id="msg-0">
                          {msg}
                        </p>
                    </div>
                </div>
            </div>
        </article>
      
         {/if}    

            
        {/each}


    </section>
    <form class="chat-input" onsubmit="return false;">
        <input type="text" autocomplete="on" placeholder="Type a message"   bind:value={usrMsg} />
        <button on:click={sendMessage}>
            Send
        </button>
    </form>
</section>



<style>

    ::-webkit-scrollbar {
        width: 4px;
    }
    ::-webkit-scrollbar-thumb {
        background-color: #4c4c6a;
        border-radius: 2px;
    }
    .chatbox {
        height: 100vh;
        display: flex;
        flex-direction: column;
        overflow: hidden;
        box-shadow: 0 0 4px rgba(0, 0, 0, 0.14), 0 4px 8px rgba(0, 0, 0, 0.28);
    }
    .chat-window {
        flex: auto;
        padding: 10px;
        max-height: calc(100% - 60px);
        background: #2f323b;
        overflow: auto;
    }
    .chat-input {
        flex: 0 0 auto;
        height: 60px;
        background: #40434e;
        border-top: 1px solid #2671ff;
        box-shadow: 0 0 4px rgba(0, 0, 0, 0.14), 0 4px 8px rgba(0, 0, 0, 0.28);
    }
    .chat-input input {
        height: 59px;
        line-height: 60px;
        outline: 0 none;
        border: none;
        width: calc(100% - 60px);
        color: white;
        text-indent: 10px;
        font-size: 12pt;
        padding: 0;
        background: #40434e;
    }
    .chat-input button {
        float: right;
        outline: 0 none;
        border: none;
        background: #2671ff;
        height: 40px;
        width: 40px;
        border-radius: 20%;
        margin: 10px;
        transition: all 0.15s ease-in-out;
        color: white;
        font-weight: bold;
    }
  
    .msg-container {
        position: relative;
        display: inline-block;
        width: 100%;
        margin-left: 10px;
        margin-right: 10px;
        margin-bottom: 10px;
        margin-bottom: 2px;
        padding: 0;
    }
    .msg-box {
        display: flex;
        background: #5b5e6c;
        
        margin-left: 10px;
        margin-right: 10px;
        margin-bottom: 10px;
        border-radius: 0 6px 6px 0;
        max-width: 80%;
        width: auto;
        float: left;
        box-shadow: 0 0 2px rgba(0, 0, 0, 0.12), 0 2px 4px rgba(0, 0, 0, 0.24);
    }
    .user-img {
        display: inline-block;
        border-radius: 50%;
        height: 40px;
        width: 40px;
        background: #2671ff;
        margin: 0 10px 10px 0;
    }
    .flr {
        flex: 1 0 auto;
        display: flex;
        flex-direction: column;
        width: calc(100% - 50px);
    }
    .messages {
        flex: 1 0 auto;
    }
    .msg {
        display: inline-block;
        font-size: 11pt;
        line-height: 13pt;
        color: white;
        font-weight: bold;
        padding: 7px;
        margin: 0 0 4px 0;
    }
    .msg:first-of-type {
        margin-top: 8px;
    }
    .timestamp {
        color: rgba(0, 0, 0, 0.38);
        font-size: 8pt;
        margin-bottom: 10px;
    }
    .username {
        margin-right: 3px;
    }
    .posttime {
        margin-left: 3px;
    }
    .msg-self .msg-box {
        border-radius: 6px 0 0 6px;
        background: #2671ff;
        float: right;
    }
    .msg-self .user-img {
        margin: 0 0 10px 10px;
    }
    .msg-self .msg {
        text-align: right;
    }
    .msg-self .timestamp {
        text-align: right;
    }
</style>
