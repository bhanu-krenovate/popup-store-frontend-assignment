<template>
    <Sidebar>
        <template #header>
          <TitleIcon :icon="'angle-left'" :title="'Make Https Request'" @iconclick="iconclick"/>
          <div @click="togglePopup">
            <font-awesome-icon :icon="['fas', 'ellipsis']" class="icon alt"/>
          </div>
            <ActionPopup @delete="handleDelete" @inactive="handleInactive" @close="closePopup" v-if="popup"/>
      </template>
        
    </Sidebar>
</template>

<script>
import SelectSingleAction from "@/components/actions/SelectSingleAction.vue"
import TitleIcon from "@/components/ui/TitleIcon.vue";
import Sidebar from "@/components/ui/Sidebar.vue";
import ActionPopup from "@/components/actions/ActionPopup.vue";

import { debounce } from "debounce";
import { mapActions, mapGetters } from 'vuex';

export default {
    data() {
        return {
            popup: false,
        }
    },
    components: {
        SelectSingleAction,
        Sidebar,
        TitleIcon,
        ActionPopup
    },
    mounted() {
    },
    methods : {
        ...mapActions({
            updateActionState: 'updateActionState',
            addAction: 'actions/addAction',
            deleteAction: 'actions/deleteAction',
            inactiveAction: 'actions/inactiveAction',
        }),
        handleDelete(e) {
            this.deleteAction(5);
            this.updateActionState('AddAction')
        },
        handleInactive(e) {
            this.inactiveAction(5)
            this.updateActionState('AddAction')
        },
        togglePopup() {
            this.popup = !this.popup
        },
        iconclick() {
            this.updateActionState('AddAction')
        },
        closePopup() {
            this.popup = false;
        }
    },
    computed: {
        ...mapGetters({
            actions: 'actions/actions',  
        }),
        nativeFilteredActions() {
            return this.filterActions.filter((action) => {
                return action.type === 'native';
            })
        },
        externalFilteredActions() {
            return this.filterActions.filter((action) => {
                return action.type === 'external';
            })
        }        
    }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/actions/SearchAction';
</style>