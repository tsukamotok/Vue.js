<template>
  <div class="container">
    <!-- 親のpropsを受け取る -->
    <h1 class="text-center pb-2">{{ttl}}</h1>
    <!-- submitイベントを無効に -->
    <form @submit.prevent>
      <div class="form-group">
        <label for="name">名前</label>
        <input
          type="text"
          class="form-control"
          id="name"
          v-model="data.name"
          @change="createMessage()"
        />
      </div>
      <div class="form-group">
        <label for="reason">遅刻理由</label>
        <select class="form-control" v-model="data.reason" id="reason" @change="createMessage()">
          <option v-for="(reason, index) in reasons" :key="index">{{reason}}</option>
        </select>
      </div>
      <!-- 理由がその他の時のみ表示 -->
      <div class="form-group" v-show="data.reason=='その他'">
        <input
          type="text"
          class="form-control"
          id="reson_other"
          @change="createMessage()"
          placeholder="その他の場合は理由を書いてください。"
          v-model="data.reason_other"
        />
      </div>
      <div class="form-group">
        <label>種別</label>
        <div class="form-check" v-for="(type, index) in types" :key="index">
          <input
            class="form-check-input"
            type="radio"
            v-model="data.schedule"
            :id="index"
            :value="type"
            :checked="((index!=0)? '': 'checked')"
            @change="createMessage()"
          />
          <label class="form-check-label" :for="index">{{type}}</label>
        </div>
      </div>
      <!-- 種別が遅刻しますの時のみ表示 -->
      <div class="form-group" v-show="data.schedule == '遅刻します'">
        <label>遅刻時間</label>
        <div class="form-row align-items-center">
          <div class="col-5 form-group">
            <select class="form-control" v-model="data.time1" @change="createMessage()">
              <!-- createTimes()の配列を設定 -->
              <option v-for="(time, index) in createTimes(0,12)" :key="index">{{time}}</option>
            </select>
          </div>
          <div class="col-2 text-center form-group">
            <span>～</span>
          </div>
          <div class="col-5 form-group">
            <!-- createTimes()の配列を設定 -->
            <select class="form-control" v-model="data.time2" @change="createMessage()">
              <option v-for="(time, index) in createTimes(1,13)" :key="index">{{time}}</option>
            </select>
          </div>
        </div>
      </div>
      <div class="form-group">
        <label for="message">メッセージ</label>
        <!-- createMessageで生成されたデータを挿入 -->
        <textarea class="form-control" id="message" rows="5" v-model="data.message"></textarea>
      </div>
      <div class="form-group pt-2">
        <button class="btn btn-primary btn-lg btn-block" id="send" type="submit">送信</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "InputForm",
  props: {
    ttl: String
  },
  data() {
    return {
      data: {
        name: "",
        reason: "",
        reason_other: "",
        schedule: "",
        time1: "",
        time2: "",
        message: ""
      },
      // 遅刻理由の中身
      reasons: ["電車遅延", "体調不良", "忘れ物", "その他"],
      // 種別の中身
      types: [
        "遅刻します",
        "全休をいただきます",
        "午前休をいただきます",
        "午後休をいただきます"
      ],
      // メッセージのテンプレート
      text1: "おはようございます。",
      text2: "よろしくお願いします。"
    };
  },
  methods: {
    // 時間のオプションを配列で生成
    createTimes: function(init, to) {
      const times = [];
      for (let index = init; index < to; index++) {
        times.push(`${index * 5}分`);
      }
      return times;
    },
    // inputに変更があった場合にメッセージの作成
    createMessage: function() {
      let text, text_reson, text_schedule;

      // その他の場合は、reason_otherのデータを使用
      if (this.data.reason == "その他") {
        text_reson = this.data.reason_other;
      } else {
        text_reson = this.data.reason;
      }

      // 遅刻の場合は時間に入力した時間を使用して生成
      if (this.data.schedule == "遅刻します") {
        text_schedule = `${this.data.time1}～${this.data.time2}ほど${this.data.schedule}`;
      } else {
        text_schedule = this.data.schedule;
      }

      // メッセージを生成
      text = `${this.text1}${this.data.name}です。${text_reson}のため${text_schedule}。${this.text2}`;

      // データに渡す
      this.data.message = text;
    }
  }
};
</script>

<style>
</style>