<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <div class="panel panel-default" v-if="logged">
                    <div class="panel-body">
                        <p>
                            {{ $t('translation.name')}}: <strong>{{ name }}</strong>
                        </p>
                        <p>
                            {{ $t('translation.email')}}: <strong>{{ email }}</strong>
                        </p>
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
    };
</script>
