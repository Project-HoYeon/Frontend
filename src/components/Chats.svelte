<script>
    /** @type ChatRooms */
    export let chatRooms = [];

    /**
     * @typedef {ChatRoom[]} ChatRooms
     * @typedef {{name: string, lastChat: string, thumbnail: string}} ChatRoom
     */
</script>
<div class="wrapper">
    {#each chatRooms as room}
        <div class="room-wrapper">
            <img class="thumbnail" src={room.thumbnail} alt="thumbnail">
            <div>
                <p class="room-name">{room.name}</p>
                <p class="room-last-chat">{room.lastChat}</p>
            </div>
        </div>
    {/each}
</div>
<style lang="scss">
  .wrapper {
    width: 100%;
    padding: 0 1rem;
    box-sizing: border-box;

    .room-wrapper {
      display: flex;
      align-items: center;
      padding: 0.4rem 0;
    }

    div + div {
      border-top: 1px solid #e8e8e8;
    }

    img {
      border-radius: 0.625rem;
      width: 3.2rem;
      height: 3.2rem;
      flex-shrink: 0;
      margin-right: 0.6rem;
      border: solid 2px #ededed;
      box-sizing: border-box;
    }

    .room-name {
      color: #000;
      font-size: 1.2rem;
      font-style: normal;
      font-weight: 700;
      line-height: normal;
    }

    .room-last-chat {
      color: #000;
      font-size: 0.9rem;
      font-style: normal;
      font-weight: 400;
      line-height: normal;
    }
  }
</style>