<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div class="panel panel-default">
                    <div class="panel-heading">
                        <h3 class="panel-title">{{ $t("translation.login") }}</h3>
                    </div>
                    <div class="panel-body">
                        <div v-if="progress" class="loading"></div>
                        <div class="col-md-12">
                            <form class="form-horizontal" @submit.prevent="auth">
                                <div :class="{ 'form-group': true, 'has-error': errors.has('email') }">
                                    <label for="email" class="control-label">{{ $t("translation.email") }}</label>
                                    <input id="email" type="email" class="form-control" name="email" v-model="email" v-validate="'required|email|max:255'" autofocus>
                                    <span v-show="errors.has('email')" class="help-block">{{ errors.first('email') }}</span>
                                </div>
                                <div :class="{ 'form-group': true, 'has-error': errors.has('password') }">
                                    <label for="password" class="control-label">{{ $t("translation.password") }}</label>
                                    <input id="password" type="password" class="form-control" name="password" v-model="password" v-validate="'required|min:6'" @input="$validator.validate('password', 1)">
                                    <span v-show="errors.has('password')" class="help-block">{{ errors.first('password') }}</span>
                                </div>
                                <div class="form-group">
                                    <button type="submit" class="login-btn btn btn-success" :disabled="this.progress">
                                        {{ $t("translation.login") }}
                                    </button>
                                    <router-link :to="{ name: 'auth.password.email' }" class="btn btn-link">
                                        {{ $t("translation.forgotPassword") }}
                                    </router-link>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import userMixin from '../../mixins/user';

    export default {
        mixins: [
            userMixin,
        ],
        methods: {
            auth() {
                this.$validator.validateAll();

                if (!this.errors.any()) {
                    this.progress = true;

                    this.$store.dispatch('login', {
                        data: {
                            email: this.email,
                            password: this.password,
                        },
                        success: () => {
                            this.$router.push(this.$route.query.redirect || { name: 'home' });
                        },
                        error: () => {
                            this.$validator.validate('password', 0);

                            this.progress = false;
                        },
                    });
                }
            },
        },
        data() {
            return {
                progress: false,
            };
        },
        created() {
            this.$validator.attach('password', 'login');
        },
        beforeDestroy() {
            this.password = '';
        },
    };
</script>
