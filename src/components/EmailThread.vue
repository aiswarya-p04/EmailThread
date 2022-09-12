<template>
  <div v-if="onRep">
    <div class="reply">
      <div class="sub">Re(5): The iPhone 14 and 14 Plus are official</div>
      <div class="from"><span class="sp-from">To: </span>{{ result.to }}</div>
      <textarea id="rep-txt" name="rep-txt" rows="10" cols="130"></textarea>
      <div class="send">
        <button class="b-send" v-on:click="sendAll">Send</button>
        <button v-on:click="onRep = false" class="b-cancel">Cancel</button>
      </div>
    </div>
    <hr />
  </div>
  <div v-if="onReplyy">
    <div class="reply">
      <div class="sub">Re(5): The iPhone 14 and 14 Plus are official</div>
      <div class="from"><span class="sp-from">To: </span>{{ result.from }}</div>
      <textarea id="rep-txt" name="rep-txt" rows="10" cols="130"></textarea>
      <div class="send">
        <button class="b-send" v-on:click="send">Send</button>
        <button v-on:click="onReplyy = false" class="b-cancel">Cancel</button>
      </div>
    </div>
    <hr />
  </div>
  <div v-if="onFwd">
    <div class="reply">
      <div class="sub">Re(5): The iPhone 14 and 14 Plus are official</div>
      <div class="from">
        <div class="input-email">
          <span class="sp-from">To: <input type="email" /></span>
        </div>
        <div class="input-email">
          <span class="sp-from">CC: <input type="email" /></span>
        </div>
      </div>
      <textarea id="rep-txt" name="rep-txt" rows="10" cols="130"></textarea>
      <div class="send">
        <button class="b-send" v-on:click="(fwd = true), (onFwd = false)">
          Send
        </button>
        <button v-on:click="onFwd = false" class="b-cancel">Cancel</button>
      </div>
    </div>
    <hr />
  </div>
  <div class="e-thread" v-if="onDel4">
    <div class="subject">
      {{ result.subject }} <span v-if="txt" style="color: blue"> &#10548; </span
      ><span v-if="fwd" style="color: blue"> &#8594; </span>
    </div>
    <div class="all">
      <div class="from-to">
        <div class="from">
          <span class="sp-from">From: </span>{{ result.from }}
        </div>
        <div class="from"><span class="sp-from">To: </span>{{ result.to }}</div>
      </div>
      <div class="timing">
        <div class="btn">
          <button v-on:click="onReplyFr">Reply</button>
          <button v-on:click="onReply">Reply All</button>
          <button v-on:click="onForward">Forward</button>
          <button v-on:click="onDelete4">Delete</button>
        </div>
        <div>Created: {{ result.created }}</div>
        <div>Sent: {{ result.sent }}</div>
        <div>Read: {{ result.read }}</div>
      </div>
    </div>
    <div class="e-body">
      {{ result.body }}
    </div>
    <hr />
  </div>

  <li v-for="item in result.conversationHistory" :key="item.messageId">
    <div class="e-thread" v-if="onDel">
      <div class="subject-con">
        {{ item.subject }}
      </div>
      <div class="all">
        <div class="from-to">
          <div class="from">
            <span class="sp-from">From: </span>{{ item.from }}
          </div>
          <div class="from"><span class="sp-from">To: </span>{{ item.to }}</div>
          <div class="from"><span class="sp-from">CC: </span>{{ item.cc }}</div>
        </div>

        <div class="timing">
          <div>
            <button v-on:click="onReplyFr">Reply</button>
            <button v-on:click="onReply">Reply All</button>
            <button v-on:click="onForward">Forward</button>
            <button v-on:click="onDelete(item.messageId)">Delete</button>
          </div>
          <div class="action">Action: {{ item.action }}</div>
          <div>Sent: {{ item.sent }}</div>
        </div>
      </div>
      <div class="e-body">
        {{ item.body }}
      </div>
      <div v-if="item.attachments.length > 0">
        {{ item.attachments }}
      </div>
    </div>
    <hr />
  </li>
</template>
<script>
import axios from "axios";
export default {
  name: "EmailThread",
  data() {
    return {
      result: "",
      onDel4: true,
      onDel: true,
      onRep: false,
      onReplyy: false,
      onFwd: false,
      txt: false,
      fwd: false,
    };
  },
  mounted() {
    axios
      .get(
        `http://sa-test-task-2022.s3-website.eu-north-1.amazonaws.com/messages`
      )
      .then((response) => (this.result = response.data));
  },
  methods: {
    onDelete4() {
      this.onDel4 = !this.onDel4;
    },
    onDelete(messageId) {
      const newVal = Object.values(this.result.conversationHistory).filter(
        (ele) => ele.messageId != messageId
      );

      this.result = { ...this.result, conversationHistory: newVal };
    },
    onReply() {
      this.onRep = !this.onRep;
    },
    onForward() {
      this.onFwd = !this.onFwd;
    },
    onReplyFr() {
      this.onReplyy = !this.onReplyy;
    },
    sendAll() {
      (this.txt = true), (this.onRep = false);
    },
    send() {
      (this.txt = true), (this.onReplyy = false);
    },
  },
};
</script>

<style scoped>
.reply {
  border-style: solid;
  border-radius: 10px;
  padding-bottom: 10px;
}
.btn {
  padding: 10px;
}
.send {
  text-align: left;
  padding-left: 100px;
}
.b-send {
  border-radius: 20px;
  background-color: green;
  color: white;
  padding: 10px;
  margin-right: 20px;
  cursor: pointer;
}
.b-cancel {
  border-radius: 20px;
  background-color: red;
  color: white;
  padding: 10px;
  cursor: pointer;
}
.sub {
  text-align: left;
  padding: 10px;
  font-weight: bold;
}
.e-thread {
  padding: 20px;
}
.subject {
  font-size: 25px;
  font-weight: bold;
  text-align: left;
  padding: 10px;
}
.input-email {
  padding: 10px;
}
.subject-con {
  font-size: 15px;
  font-weight: bold;
  text-align: left;
  padding: 10px;
}
.sp-from {
  font-weight: bold;
  font-size: 12px;
}
.from {
  text-align: left;
  font-size: 15px;
  padding: 10px;
}
.all {
  display: flex;
}
.from-to {
  flex: 1;
}
.timing {
  flex: 1;
  text-align: right;
  padding-right: 20px;
  font-size: 12px;
}
.e-body {
  text-align: left;
  padding: 30px 30px 30px 10px;
}
li {
  list-style-type: none;
}
.convo {
  text-align: left;
}
.action {
  font-weight: bold;
  padding: 10px;
}
</style>
