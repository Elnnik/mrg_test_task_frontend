<template>
  <div class="question-card">
    <p>{{questionIndex}}. {{quizzQuestion.question}}</p>
<!--    <p>{{quizzQuestion._id}}. {{quizzQuestion.question}}</p>-->

    <b-form-select
      v-model="selected_answer"
      v-if="questionType === 'singleSelect'"
      v-on:change="singleSelectResult(selected_answer)"
    >
      <b-form-select-option :value="null" disabled>-- Выберите один вариант ответа --</b-form-select-option>
      <option
        v-for="(answerObject, index) in quizzQuestion.answers"
        :key="index"
        :value="answerObject"> {{ answerObject.answer }}
      </option>
    </b-form-select>

    <b-form-select
      v-model="selected_answer"
      v-else-if="questionType === 'multipleSelect'"
      v-on:change="multipleSelectResult(selected_answer)"
      multiple :select-size="3" style="height: 150px">
      <option
        v-for="(answerObject, index) in quizzQuestion.answers"
        :key="index"
        :value="answerObject"> {{ answerObject.answer }}
      </option>
    </b-form-select>


    <v-multiselect-listbox
      v-else-if="questionType === 'multipleSelectDualbox'"
      v-model="multiSelectDualboxAnswers"
      :options="quizzQuestion.answers"
      :reduce-display-property="(option) => option.answer"
      :reduce-value-property="(option) => option._id"
      v-on:change="multiSelectDualboxResult(multiSelectDualboxAnswers)"
      selected-no-options-text="Выберите один или несколько вариантов ответа"
      no-options-found-text="Нет такого ответа"
      no-selected-options-found-text="Выбранный ответ не найден"

    >
    </v-multiselect-listbox>

    <div class="drag-and-drop-question row-fluid" v-else-if="questionType === 'dragAndDrop'">
      <div class="border border-primary justify-content-center bg-primary text-white">
        <p class="text-center">Варианты ответов: </p>
        <draggable v-model="quizzQuestion.answers" :options="{group: 'answers'}">
          <div class="list-group-item card-header text-dark"
               v-for="(answerObject, index) in quizzQuestion.answers"
               :key="index"
          >
            {{ answerObject.answer }}
          </div>
        </draggable>
      </div>
      <br>
      <br>
      <div class="border border-success justify-content-center bg-success text-white">
        <p class="text-center">Перетащите нужный ответ(ы) сюда:</p>
        <draggable v-model="dragAndDropAnswers"
                   :options="{group: 'answers'}"
                   v-on:change="dragAndDropSelectResult(dragAndDropAnswers)"
        >
          <div class="list-group-item text-dark"
               v-for="(answerObject, index) in dragAndDropAnswers"
               :key="index"
          >
            {{ answerObject.answer }}
          </div>
        </draggable>
      </div>
    </div>

<!--    <div class="mt-3">Selected: <strong>{{ selected_answer }}</strong></div>-->
    <hr>
  </div>
</template>

<script>
import Vue from 'vue'
import vMultiselectListbox from 'vue-multiselect-listbox'
import draggable from 'vuedraggable'
import 'vue-multiselect-listbox/dist/vue-multi-select-listbox.css';


Vue.component('v-multiselect-listbox', vMultiselectListbox)


export default {
  name: "questionCard",
  components: {draggable},

  props: {
    quizzQuestion: {
      type: Object,
      default() {
        return {}
      }
    },
    questionType: {
      type: String,
      default() {
        return undefined;
      }
    },
    questionIndex: {
      type: Number,
      default() {
        return undefined
      }
    }
  },

  methods: {
    singleSelectResult(answerObject) {
      console.log('SINGLE SELECT BEFORE EMMIT: ', answerObject)
      let answerObjectsList = []
      answerObjectsList.push(answerObject)
      this.$emit('singleSelectResult', {
        answerDetails: {
          questionId: this.quizzQuestion._id,
          answerObjectsList: answerObjectsList
        }
      })
    },

    multipleSelectResult(answerObjectsList) {
      console.log('MULTIPLE SELECT BEFORE EMMIT: ', answerObjectsList)
      this.$emit('multipleSelectResult', {
        answerDetails: {
          questionId: this.quizzQuestion._id,
          answerObjectsList: answerObjectsList
        }
      })
    },

    dragAndDropSelectResult(dragAndDropAnswerObjectsList) {
      console.log('BEFORE EMMIT: ', dragAndDropAnswerObjectsList)
      this.$emit('dragAndDropSelectResult', {
        answerDetails: {
          questionId: this.quizzQuestion._id,
          answerObjectsList: dragAndDropAnswerObjectsList
        }
      })
    },

    multiSelectDualboxResult(multiSelectDualboxAnswers) {
      console.log('DUALBOX BEFORE EMIT: ', this.quizzQuestion._id, multiSelectDualboxAnswers)

      let answerObjectsList = []

      multiSelectDualboxAnswers.forEach(answerId => {
        this.quizzQuestion.answers.forEach(answerObject => {
          if (answerId === answerObject._id) answerObjectsList.push(answerObject)
        })
      })

      this.$emit('multiSelectDualboxResult', {
        answerDetails: {
          questionId: this.quizzQuestion._id,
          answerObjectsList: answerObjectsList
        }
      })
    }
  },

  data() {
    return {
      selected_answer: null,
      questionId: null,
      dragAndDropAnswers: [],
      plainTextAnswer: '',
      multiSelectDualboxAnswers: []
    }
  }
}
</script>

<style scoped>

</style>
