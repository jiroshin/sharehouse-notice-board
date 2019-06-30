<template>
  <div id='form-wrapper'>
    <h3 style='letter-spacing:0.07em;margin:30px 0px;'>新宿暮らしにも慣れてきた?</h3>
    <transition name='post-alert'>
      <b-alert v-show='post_message' variant="success" show>内容が投稿されました！</b-alert>
    </transition>
    <transition name='post-validataion'>
      <b-alert v-show='post_validation' variant="danger" show>内容を入力して下さい</b-alert>
    </transition>
    <b-card
      title="なんでも書いてね"
      img-src="https://sugitashinjiro.fun/wp-content/uploads/2018/05/imgp6363.jpg"
      img-alt="Image"
      img-top
      tag="article"
      style="max-width: 20rem;"
      id="form-card"
    >
      <b-card-text>
        <div id='spinner' v-show='spinner'>
          <b-spinner variant="success" label="Spinning"></b-spinner>
        </div>
        <b-form>
          <div v-show='card_form_cont'>

            <b-form-group
              id="input-group-name"
            >
            <b-form-input
              id="input-name"
              v-model="form_data.name"
              required
              placeholder='なまえ'
            ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-cont_type" label="どんな内容？" label-for="input-3">
              <b-form-select
                id="input-cont_type"
                v-model="form_data.cont_type"
                :options="cont_types"
                required
              ></b-form-select>
            </b-form-group>

            <b-form-group
              id="input-group-cont"
            >
            <b-form-textarea
              id="input-cont"
              v-model="form_data.cont"
              placeholder="なにを伝えたいの？"
              rows="3"
              max-rows="6"
              required
            ></b-form-textarea>
            </b-form-group>

          </div>
          <b-button v-on:click='submit_form' variant="outline-success">送信！</b-button>
        </b-form>
      </b-card-text>
    </b-card>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'form-wrapper',
  data() {
    return {
    form_data: {
      name: '',
      cont_type: 'news',
      cont: '',
    },
    cont_types: [{ text: 'みんなに知らせたい！', value: 'news'}, { text: 'これ買ってきて！', value: 'shopping'}],
    id_last: '0',
    post_message: false,
    post_validation: false,
    spinner: false,
    card_form_cont: true
    }
  },
  created:
  function() {
    firebase.firestore().collection("posted_data").get().then(function(querySnapshot) {
        querySnapshot.forEach(function(doc) {

          if (Number(doc.data().id) > Number(this.id_last)){
            this.id_last = doc.data().id
          }

        }.bind(this));
    }.bind(this));
  },
  methods: {
    submit_form: function() {
      if (this.form_data.name=='' || this.form_data.cont==''){
        this.post_validation = true;
        setTimeout(function(){this.post_validation = false;}.bind(this), 2000);
        return
      };

      this.id_last = String( Number(this.id_last) + 1 );

      firebase.firestore().collection("posted_data").add({
          id: this.id_last,
          name: this.form_data.name.trim(),
          cont_type: this.form_data.cont_type.trim(),
          cont: this.form_data.cont.trim()
      });

      this.form_data.name = ''
      this.form_data.cont_type = 'news'
      this.form_data.cont= ''

      this.card_form_cont = false;
      this.spinner = true;
      setTimeout(function(){this.card_form_cont = true}.bind(this), 700);
      setTimeout(function(){this.spinner = false;}.bind(this), 700);

      this.post_message = true;
      setTimeout(function(){this.post_message = false;}.bind(this), 2000);
    }
  }
}
</script>

<style>
#form-card {
  margin: 0 auto;
}
#spinner {
  height:180px;
  padding-top:90px;
}

.post-alert-enter-active, .post-alert-leave-active {
  transition: opacity 2s, transform 1.5s;
}
.post-alert-enter {
  opacity: 0;
  transform: translateY(-50px);
}
.post-alert-leave-to {
  opacity: 0;
  transform: translateY(200px);
}

.post-validataion-enter-active, .post-validataion-leave-active {
  transition: opacity 1s, transform 1s;
}
.post-validataion-enter, .post-validataion-leave-to {
  opacity: 0;
  transform: translateY(-50px);
}
</style>
