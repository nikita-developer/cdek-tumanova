<template>
    <div>
        <h4 class="mb-3">Отследить отправление</h4>
        <b-input-group
            size="lg"
            class="mb-3"
            prepend="Номер посылки"
        >
            <b-form-input></b-form-input>
            <b-input-group-append >
                <b-button size="sm" text="Button" variant="success" @click="handleSubmit">Отследить</b-button>
            </b-input-group-append>
        </b-input-group>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        setup() {
            const handleSubmit = () => {
                axios.get('http://api.cdek.ru/v2/deliverypoints?weight_max=50&city_code=270&allowed_cod=1', {
                    headers: {
                        'Content-Type': 'application/json;charset=utf-8',
                        'Authorization': 'Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzY29wZSI6WyJvcmRlcjphbGwiLCJwYXltZW50OmFsbCJdLCJleHAiOjE2NzUwOTcwNjYsImF1dGhvcml0aWVzIjpbImNvbnRyYWdlbnQtdXVpZDoxZTdjYzY5NC03Y2EwLTQ2OTItODIzZi00MTlhNWU1Yzc1NjQiLCJjb250cmFjdDrQodCXLdCg0KQtU0hDSExLMTEtMSIsImFjY291bnQtbGFuZzpydXMiLCJhY2NvdW50LXV1aWQ6ZGU4MDg0MzgtZWNkNy00M2U0LWE2ODMtNGRhMDEyZWRlZmFmIiwiYXBpLXZlcnNpb246Mi4wIiwic2hhcmQtaWQ6cnUtMDIiLCJmdWxsLW5hbWU60JrQuNGB0LXQu9C10LIg0JDQu9C10LrRgdC10Lkg0JLQu9Cw0LTQuNC80LjRgNC-0LLQuNGHIiwiY2xpZW50LWlkLWVjNToxZTdjYzY5NC03Y2EwLTQ2OTItODIzZi00MTlhNWU1Yzc1NjQiLCJzb2xpZC1hZGRyZXNzOmZhbHNlIiwiY2xpZW50LWNpdHk60JTQvtC70LPQvtC_0YDRg9C00L3Ri9C5LCDQnNC-0YHQutC-0LLRgdC60LDRjyDQvtCx0LvQsNGB0YLRjCIsImNsaWVudC1pZC1lYzQ6bnVsbCJdLCJqdGkiOiJjMDgwYjMyYi0xZTE2LTQxZWYtOWZkMy03YzFkZWRmZTkwMTkiLCJjbGllbnRfaWQiOiJ0YnN4aVpvT1dCbTA5RlFsVHNyT3pHR0c5bGE5SVNHVyJ9.bSaDpCL16fKYVOwnNZrQ36NafE9mMnsthYfmQCPOBGk1vCIO2UKNHLEfHoI6txGxskJsQudYTZKCdJsXN-63-YxnDfpox5-jiCxdcLpOsWojHez-TS70JuixsldY0QdFKEM5sYabZPbIgrMGvirAhpNY9AjLC5qIitWRebRcvK3RBXXYZmYbjDcRnOGXOW8pqqOPgPKXGfERXxWSomV2fyzeu3w4lQJeDGZ0U7r1-AWwctosPsOGMDYJdmIesZbt5SOzU3XUsSN74L14fRW0tOkMH_07uE12VT_Cgy9K2a1ndZk-csxfBELdNpl5dldGIE0iwpVH4mtBTz3ZL0S-sw',
                    },

                })
                    .then((response) => {
                        console.log(response)
                    })
                    .catch((error) => {
                        console.log(error)
                    });
            }

            return {
                handleSubmit
            }
        }
    }
</script>

<style scoped>

</style>
