<template>
    <form class="card auth-card" @submit.prevent="submitHandler">
        <div class="card-content">
            <span class="card-title">Home CRM</span>

<!--  EMAIL-->
            <div class="input-field">
                <input
                        id="email"
                        type="text"
                        v-model.trim="email"
                        :class="{invalid:($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"

                >
                <label for="email">Email</label>
                <small
                        class="helper-text invalid"
                        v-if="$v.email.$dirty && !$v.email.required"
                >Поле не должно быть пустым</small>
                <small
                        class="helper-text invalid"
                        v-else-if="$v.email.$dirty && !$v.email.email"
                >Введите корректный e-mail</small>
            </div>


<!-- PASSWORD-->
            <div class="input-field">
                <input
                        id="password"
                        type="password"
                        v-model.trim="password"
                        :class="{invalid:($v.password.$dirty && !$v.password.required) || ($v.password.$dirty && !$v.password.minLength)}"

                >
                <label for="password">Пароль</label>
                <small
                        class="helper-text invalid"
                        v-if="$v.password.$dirty && !$v.password.required"
                >Поле не должно быть пустым</small>
                <small
                        class="helper-text invalid"
                        v-else-if="$v.password.$dirty && !$v.password.minLength"
                >Длина пароля должна быть не менее {{$v.password.$params.minLength.min}} символов</small>
            </div>


<!--NAME-->
            <div class="input-field">
                <input
                        id="name"
                        type="text"
                        v-model.trim="name"
                        :class="{invalid:($v.name.$dirty && !$v.name.required) || ($v.name.$dirty && !$v.name.minLength)}"

                >
                <label for="name">Имя</label>
                <small
                        class="helper-text invalid"
                        v-if="$v.name.$dirty && !$v.name.required"
                >Поле не должно быть пустым</small>
                <small
                        class="helper-text invalid"
                        v-else-if="$v.name.$dirty && !$v.name.minLength"
                >Длина имени должна быть не менее {{$v.name.$params.minLength.min}} символов</small>
            </div>

<!-- CHECKBOX-->
            <p>
                <label>
                    <input
                            type="checkbox"
                            v-model="agree"
                    />
                    <span

                    >
                        С правилами согласен</span>
                </label>
            </p>
        </div>
        <div class="card-action">
            <div>
                <button
                        class="btn waves-effect waves-light auth-submit"
                        type="submit"
                >
                    Зарегистрироваться
                    <i class="material-icons right">send</i>
                </button>
            </div>

            <p class="center">
                Уже есть аккаунт?
                <router-link to="/login">Войти!</router-link>
            </p>
        </div>
    </form>
</template>

<script>
    import {email, required, minLength } from 'vuelidate/lib/validators'
    export default {
        metaInfo:{
            title: 'Регистрация'
        },
        name: 'register',
        data: () => ({
            email:'',
            password:'',
            name:'',
            agree: false
        }),
        methods:{
           async submitHandler(){
                if(this.$v.$invalid){
                    this.$v.$touch()
                    return
                }
                const formData = {
                    name: this.name,
                    email: this.email,
                    password: this.password
                }
                try {
                    await this.$store.dispatch('register', formData)
                    this.$router.push('/')
                }catch (e) {}
            }
        },
        validations:{
            email:{email, required},
            password: {required, minLength: minLength(7)},
            name: { required, minLength:minLength(5)},
            agree:{checked: v => v}
        }

    }
</script>