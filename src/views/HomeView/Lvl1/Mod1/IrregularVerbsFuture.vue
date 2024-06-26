<script setup lang="ts">
import useViewStore from '@/stores/ViewStore'
import useTestStore from '@/stores/TestStore'
import ProgressHeader from '@/components/ProgressHeader.vue'
import { onBeforeRouteLeave } from 'vue-router'
import { ref, computed } from 'vue'

const LESSON_NAME = 'Irregular verbs - Future'
const LESSON_PARTS = 3

// importing component functions from ProgressHeader
const child = ref<InstanceType<typeof ProgressHeader> | null>(null)
const incrementProgress = () => child.value?.incrementProgress()
const decrementProgress = () => child.value?.decrementProgress()
const currentPart = computed(() => {
    return (child.value as InstanceType<typeof ProgressHeader>)?.currentPart
})

const viewStore = useViewStore()
const testStore = useTestStore()

onBeforeRouteLeave((to) => {
    const toPath = to.path
    const previousPath = /^\/level-\d+\/module-\d+$/
    if (previousPath.test(toPath)) {
        viewStore.mainNavVisible = true
    }

})

function updateAllData() {
    testStore.updateStreak()
    testStore.testResults
        .tests['level-1']
        .modules['module-1']
        .lessons['irregular-verbs-future']
        .completed = true

    testStore.setTestResults()
}
</script>
<template>
    <div class="lesson-view-wrapper">
        <ProgressHeader :headerName="LESSON_NAME" :totalParts="LESSON_PARTS" ref="child" />
        <main>
            <div v-if="currentPart === 1">
                <h1>Būt (to be)</h1>
                <p>
                    The table below shows the verb "būt" conjugations in future tense.
                </p>
                <table>
                    <tr>
                        <th>Pronoun</th>
                        <th>Future tense</th>
                    </tr>
                    <tr>
                        <td>es</td>
                        <td>būšu</td>
                    </tr>
                    <tr>
                        <td>tu</td>
                        <td>būsi</td>
                    </tr>
                    <tr>
                        <td>viņš, viņa</td>
                        <td>būs</td>
                    </tr>
                    <tr>
                        <td>mēs</td>
                        <td>būsim</td>
                    </tr>
                    <tr>
                        <td>jūs</td>
                        <td>būsit</td>
                    </tr>
                    <tr>
                        <td>viņi, viņas</td>
                        <td>būs</td>
                    </tr>
                </table>
            </div>
            <div v-if="currentPart === 2">
                <h1>Dot (to give)</h1>
                <p>
                    The table below shows the verb "dot" conjugations in future tense.
                </p>
                <table>
                    <tr>
                        <th>Pronoun</th>
                        <th>Future tense</th>
                    </tr>
                    <tr>
                        <td>es</td>
                        <td>došu</td>
                    </tr>
                    <tr>
                        <td>tu</td>
                        <td>dosi</td>
                    </tr>
                    <tr>
                        <td>viņš, viņa</td>
                        <td>dos</td>
                    </tr>
                    <tr>
                        <td>mēs</td>
                        <td>dosim</td>
                    </tr>
                    <tr>
                        <td>jūs</td>
                        <td>dosit</td>
                    </tr>
                    <tr>
                        <td>viņi, viņas</td>
                        <td>dos</td>
                    </tr>
                </table>
            </div>
            <div v-if="currentPart === 3">
                <h1>Iet (to go)</h1>
                <p>
                    The table below shows the verb "iet" conjugations in future tense.
                </p>
                <table>
                    <tr>
                        <th>Pronoun</th>
                        <th>Future tense</th>
                    </tr>
                    <tr>
                        <td>es</td>
                        <td>iešu</td>
                    </tr>
                    <tr>
                        <td>tu</td>
                        <td>iesi</td>
                    </tr>
                    <tr>
                        <td>viņš, viņa</td>
                        <td>ies</td>
                    </tr>
                    <tr>
                        <td>mēs</td>
                        <td>iesim</td>
                    </tr>
                    <tr>
                        <td>jūs</td>
                        <td>iesit</td>
                    </tr>
                    <tr>
                        <td>viņi, viņas</td>
                        <td>ies</td>
                    </tr>
                </table>
            </div>
        </main>
        <footer>
            <button v-if="currentPart > 1" type="button" class="btn-back" @click="decrementProgress">
                Back
            </button>
            <button
                v-if="currentPart !== LESSON_PARTS"
                type="button"
                class="btn-next"
                @click="incrementProgress"
            >
                Next
            </button>
            <RouterLink v-if="currentPart === LESSON_PARTS" :to="{ name: 'module-1' }" tabindex="-1">
                <button type="button" class="btn-next" @click="updateAllData">Finish</button>
            </RouterLink>
        </footer>
    </div>
</template>

<style scoped></style>
