<template>
  <div class="root">
    <div class="container">
      <form class="niceform" @submit.prevent="checkForm">
        <div class="user_data">
          <div class="line-1">
            <div class="formEl">
              <label for='name'>Имя</label>
              <input id= 'name' :class="$v.form.name.$error ? 'is_invalid' : ''" type="text" class="name"
              v-model.trim = 'form.name'>
              <p class="ermes" v-if="$v.form.name.$dirty && !$v.form.name.required" >
                Это поле обязательно для заполнения
              </p>
              <p class="ermes" v-if="$v.form.name.$dirty && !$v.form.name.minLength" >
                Количество символов должно быть не меньше 2
              </p>
            </div>
            <div class="formEl">
              <label for='lastName'>Фамилия</label>
              <input id="lastName" 
              :class="$v.form.lastName.$error ? 'is_invalid' : ''"
              type="text" 
              class="last_name"
              v-model.trim = "form.lastName"
              >
              <p class="ermes" v-if="$v.form.lastName.$dirty && !$v.form.lastName.required" >
                Это поле обязательно для заполнения
              </p>
              <p class="ermes" v-if="$v.form.lastName.$dirty && !$v.form.lastName.minLength" >
                Количество символов должно быть не меньше 2
              </p>
            </div>
          <div class="formEl">
          <label for='middleName'>Отчество</label>
          <input id="middleName" type="text" class="middle_name"
          v-model.trim="form.middleName"
          ></div>
          </div>
          <div class="formGroup">
            <div class="formEl">
              <label for='birthdate'>Дата рождения</label>
              <input 
              :class="$v.form.birthday.$error ? 'is_invalid' : ''"
              v-model="form.birthday" 
              pattern="[0-9]{2}+.+[0-9]{2}+.+[0-9]{4}" 
              placeholder="xx.xx.xxxx" 
              id="birthdate" 
              type="text" 
              class="birthdate">
              <p class="ermes" v-if="$v.form.birthday.$dirty && !$v.form.birthday.required" >
                Введите дату
              </p>
            </div>
            <div class=" formEl">
              <label for='telephone'>Номер телефона</label>
              
              <input v-model="form.telephone" pattern='[0-9]{11}'  id="telephone" type="text" class="telephone" required>
            </div>
            <div class=" formEl">
              <label for='gender'>Пол</label>
              <div id='gender' :style= "show.doc ? 'background-color: #a8a8a8' : ''" :class="show.doc ? 'focused' : ''"  @click="show.doc = !show.doc"  class="select" >
                {{form.gender}}
                <transition name = 'fade'>
                  <div v-if="show.doc" class="option_frame">
                    <div class="option"
                      @click="chooseOptG"
                      v-for="(male, index) in selectors.gender"
                      :key = 'index'
                      >{{male}}
                    </div>
                  </div>
                </transition>
              </div>
            </div>
          </div>
          <div class="formGroupdbl">
            <div class="formEl">
              <label for="cl">Группа клиентов</label>
              <div id="cl" 
              :style= "show.cl ? 'background-color: #a8a8a8' : ''"
              :class="[show.cl ? 'focused' : '', $v.form.clients.$error ? 'is_invalid' : '']"
               @click="show.cl = !show.cl" 
               class="clients select">
                Выбрано {{form.clients.length}}
                <transition name = 'fade'>
                  <div v-if="show.cl" class="option_frame">
                    <div class="option"
                      @click="chooseOptCl"
                      v-for="(client, index) in selectors.clients"
                      :key="index"
                      >{{client}}
                    </div>
                  </div>
                </transition>
              </div>
              <p class="ermes" v-if="$v.form.clients.$dirty && !$v.form.clients.required" >
                Выберите хотя бы один вариант
              </p>
              <div v-for="(cl, index) in form.clients"
          :key="index" class="selected">
          <div class="erase"
           @click="ErCl">×</div>
          <div class="clName">{{cl}}</div>
          
          </div>
            </div>
            
            <div class="formEl">
              <label for="doct">Лечащий врач</label>
               <div id="doct" :style= "show.doct ? 'background-color: #a8a8a8' : ''" 
               :class="show.doct ? 'focused' : ''" @click="show.doct = !show.doct" class="doctors select">
                {{form.doctor}}
                <transition name = 'fade'>
                  <div v-if="show.doct" class="option_frame">
                    <div class="option"
                      @click="chooseOptD"
                      v-for="(doctor, index) in selectors.doctors"
                      :key="index"
                      >{{doctor.name}}
                    </div>
                  </div>
                </transition>
              </div>
            </div>
          </div>
          
          <div class="sms">
            <input v-model="form.sms" class='check' type="checkbox" id="sendsms">
            <label for="sendsms">Не отправлять СМС</label>
          </div>
        </div>
         <div class="user-adress">
           <div class="formEl">
             <div class="title">
               Адрес
             </div>
           </div>
           <div class="formGroup">
             <div class="formEl">
               <label for='index'>Индекс</label>
              <input v-model="form.index" id="index" type="text" class="index">
             </div>
             <div class="formEl">
               <label for='country'>Страна</label>
              <input id="country" type="text" class="country"
              v-model="form.country">
             </div>
             <div class="formEl">
               <label for='area'>Область</label>
              <input v-model="form.area" id="area" type="text" class="area">
             </div>
           </div>
           <div class="formGroup">
             <div class="formEl">
               <label for='city'>Город</label>
              <input
              :class="$v.form.city.$error ? 'is_invalid' : ''"
               v-model="form.city" 
               id="city" 
               type="text" class="city">
               <p class="ermes" v-if="$v.form.city.$dirty && !$v.form.city.required" >
                 Заполните данное поле
              </p>
               <p class="ermes" v-if="$v.form.city.$dirty && !$v.form.city.alpha" >
                 Цифры недопустимы
              </p>
             </div>
             
             <div class="formEl">
               <label for='street'>Улица</label>
              <input v-model="form.street" id="street" type="text" class="street">
             </div>
             <div class="formEl">
               <label for='house'>Дом</label>
              <input v-model="form.house" id="house" type="text" class="house">
             </div>
           </div>
         </div>
         <div class="user_docs">
           <div class="formEl">
             <div class="title">
                Паспорт
             </div>
           </div>
           <div class="formGroup">
            <div class="formEl">
              <label for='docs_type'>Тип документа</label>
              <div id='docs_type' 
              :style="show.type ? 'background-color: #a8a8a8' : ''" 
              :class="[show.type ? 'focused' : '', $v.form.type.$error ? 'is_invalid' : '']" 
               @click="show.type = !show.type"  class="select" >
              {{form.type}}
              <transition name = 'fade'>
                <div v-if="show.type" class="option_frame">
                  <div id="docs" class="option"
                    @click="chooseOptT"
                    v-for="(type, index) in selectors.type"
                    :key = 'index'
                    >{{type.name}}
                  </div>
                </div>
              </transition>
            </div>
            <p class="ermes" v-if="$v.form.type.$dirty && !$v.form.type.required" >
                Заполните данное поле
              </p>
            </div>
            
            <div class="formEl">
              <label for='serial'>Серия</label>
              <input
              :class="$v.form.serial.$error ? 'is_invalid' : ''"
               v-model="form.serial" id="serial" type="text" class="serial">

              <p class="ermes" v-if="$v.form.serial.$dirty && !$v.form.serial.numeric" >
                Серия состоит из цифр
              </p>
              <p class="ermes" v-if="$v.form.serial.$dirty && !$v.form.serial.minLength" >
                Серия состоит из 4 цифр
              </p>
              <p class="ermes" v-if="$v.form.serial.$dirty && !$v.form.serial.maxLength" >
                 Серия состоит из 4 цифр
              </p>
            </div>
            <div class="formEl">
              <label for='number'>Номер</label>
              <input
              :class="$v.form.number.$error ? 'is_invalid' : ''"
               v-model="form.number" id="number" type="text" class="number">
               <p class="ermes" v-if="$v.form.number.$dirty && !$v.form.number.numeric" >
                Номер состоит из цифр
              </p>
              <p class="ermes" v-if="$v.form.number.$dirty && !$v.form.number.minLength" >
                Номер состоит из 6 цифр
              </p>
              <p class="ermes" v-if="$v.form.number.$dirty && !$v.form.number.maxLength" >
                 Номер состоит из 6 цифр
              </p>
            </div>
           </div>
           <div class="formGroupdbl">
           <div class="formEl">
             <label for='member'>Кем выдан</label>
              <input v-model="form.member" id="member" type="text" class="member">
           </div>
           <div class="formEl">
             <label for='date'>Дата выдачи</label>
              <input 
              :class="$v.form.date.$error ? 'is_invalid' : ''"
              v-model="form.date" id="date" type="text" class="date">
              <p class="ermes" v-if="$v.form.date.$dirty && !$v.form.date.required" >
                 Введите дату
              </p>
           </div>
           </div>
         </div>
      <input id="sub_btn" type="submit">
    </form>
    </div>
  </div> 
</template>

<script>

import { validationMixin } from 'vuelidate'
import { required, minLength, numeric, maxLength, alpha} from 'vuelidate/lib/validators'

export default {
  name: 'formComponent',
  mixins: [validationMixin],
  data() {
    return {
      show: {
        type: false,
        doc: false,
        doct: false,
        cl: false
      },
      form: {
        name: '',
        lastName:'', 
        middleName: '', 
        birthday: '',
        telephone: '7', 
        gender: '', 
        clients: [], 
        doctor: '', 
        sms: false,
        index: '', 
        country: '', 
        area: '',
        city: '', 
        street: '', 
        house: '', 
        type: '',  
        serial: '',
        number: '', 
        member: '', 
        date: '',
      },
      selectors: {
        gender: ['мужской', 'женский'],
        type: [
            {
              label:'passport', 
              name: 'Паспорт',
            },
            {
              label:'drives', 
              name: 'Вод. удостоверение',
            },
            {
              label:'birth', 
              name: 'Свид. о рождении',
            },
          ], 
          clients: ['VIP','Проблемные', 'ОМС', ],
          doctors: [
            {
              label: 'Ivanov ',
              name: 'Иванов',
            },
            {
              label: 'Zaharov',
              name: 'Захаров',
            },
            {
              label: 'Cherysheva',
              name: 'Черышева',
            },
          ],

      },
       
   }
  }, 
  validations:{
    form: {
      name: {
        required, 
        minLength: minLength(2)
        },
      lastName: {
          required, 
          minLength: minLength(2)
        },
      middleName: {}, 
      birthday: {
        required, 
      },
      telephone: {
        required,
        numeric,
      }, 
      clients: {
        required
      }, 
      city: {
        required, alpha
      },  
      type: {
        required
      },  
      serial: {
        numeric,
        minLength: minLength(4),
        maxLength: maxLength (4)

      },
      number: {
        numeric,
        minLength: minLength(6),
        maxLength: maxLength (6)
      }, 
      member: '', 
      date: {
        required
      },
    
    },
    
  },
  methods: {
    checkForm() {
      this.$v.form.$touch()
      if(!this.$v.form.$error) {
        alert('Ваша форма успешно отправлена')
      }
    },
    chooseOptG($e) {
      this.form.gender = $e.target.innerText
    },

    chooseOptT($e) {
      this.form.type = $e.target.innerText
    },

    chooseOptD($e) {
      this.form.doctor = $e.target.innerText
    },

    chooseOptCl($e) {
      const index = this.selectors.clients.indexOf($e.target.innerText);
      this.form.clients.push($e.target.innerText);
      this.selectors.clients.splice(index, 1)
    },

    ErCl($e) {
      const index = this.form.clients.indexOf($e.path[1].children[1].innerText); // Добираемся до соседнего с крестиком текста чтобы по значению найти его индекс
      this.form.clients.splice(index, 1) // по найденому индексу удаляем элемент массива
      this.selectors.clients.push($e.path[1].children[1].innerText) // добавляем удаленный элемент обратно в селектор
    }

  },

}
</script>

<style lang="sass">
form
  box-sizing: border-box
  font-family: 'Raleway', sans-serif
  background-color: #4f4f4f
  padding: 30px
  border-radius: 20px 
  border: 3px solid #79d4a9
  box-shadow: 0px 0px 7px 2px rgb(0,0 ,0 )
label
  margin-left: 10px
  font-size: 14px
  font-weight: 700
  color: #919191
.clName
  font-size: 14px
  align-self: center
  padding-left: 5px
  font-weight: 600
  color: #4f4f4f
.erase
  font-size: 23px
  line-height: 20px
  align-self: flex-start
  padding: 3px 5px
  color: #4f4f4f
  border-right: 2px solid #4f4f4f
  cursor: pointer

.selected
  display: flex
  background-color: #5fb86b
  margin-top: 10px
  width: 150px
  border-radius: 5px 
input, .select
  font-family: 'Raleway', sans-serif
  font-size: 16px
  text-align: left
  padding: 5px 10px 
  border: 1px solid #79d4a9  
  border-radius: 7px 
  transition: all .1s ease-out
  &:focus-visible
    box-shadow: 0px 0px 14px 2px rgba(121,212,169,0.71)
    outline: none
    border: 2px solid #79d4a9
    background-color: #a8a8a8
    color: #fff

.fade-enter-active, .fade-leave-active
  transition: opacity .1s ease-out

  
.fade-enter, .fade-leave-to  
  opacity: 0
.is_invalid
  box-shadow: 0px 0px 14px 2px rgb(255, 66, 66)
  outline: none
  border: 2px solid #ff4242
  background-color: #a8a8a8
  color: #fff
.ermes
  font-size: 12px
  margin: 0
  margin-top: 5px
  text-align: left
  color: #ff3d3d
.focused
  box-shadow: 0px 0px 14px 2px rgba(121,212,169,0.71)
  outline: none
  border: 2px solid #79d4a9
  background-color: #a8a8a8
  color: #fff
.select
  position: relative
  vertical-align: middle
  font-size: 16px
  height: 20px
  width: 130px
  cursor: pointer
  background-color: #fff
#doct .option_frame
  width: 228px
#cl .option_frame
  width: 228px
   
.option_frame
  text-align: center
  position: absolute
  z-index: 10
  background-color: #fff
  top: 30px
  left: 3px
  width: 143px
  cursor: pointer 
  border-top: 2px solid #79d4a9
  -webkit-box-shadow: inset 0px 17px 3px -15px #79d4a9
  box-shadow: inset  0px 16px 2px -15px #79d4a9, 0px 0px 2px 0px #79d4a9
.option:hover 
  cursor: pointer
  background-color: #aaa
  color: #fff
.ml
  margin-left: 10px

#docs
  font-size: 10px
#docs_type
  font-size: 12px
.option
  transition: all .1s ease-out
  padding-top: 5px 
  padding-bottom: 10px
  color: #000
.formGroup, .formGroupdbl
  display: flex
  justify-content: space-between
  flex-direction: row

.niceform
  max-width: 560px
  margin-left: auto
  margin-right: auto
.title
  text-align: left
  font-size: 18px
  font-weight: 700
  color: #919191
.formEl 
  display: flex
  margin-top: 20px
  flex-direction: column  

.formEl label
  text-align: left
  font-size: 12px 

.formGroup .formEl input
  width: 130px

.formGroupdbl .formEl .member
  width: 320px
  
.formGroupdbl .formEl .date
  width: 120px

.formGroupdbl .formEl .clients, .doctors
  
  width: 215px
.sms
 margin-top: 10px
 text-align-last: left    
#sub_btn 
  margin-top: 20px  
  background-color: #a8a8a8
  color: #5c5c5c
  font-weight: 600

@media (max-width: 480px)
  form
    width: 100%
  .formGroupdbl .formEl .member
    width: unset
  .formGroup .formEl input 
    width: unset
  .formGroupdpl .formEl input 
    width: unset
  .formGroupdbl, .formGroup
    flex-direction: column
  
</style>