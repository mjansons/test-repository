<script setup lang="ts">
import ProgressHeader from '@/components/ProgressHeader.vue'
import { onBeforeRouteLeave } from 'vue-router'
import { ref } from 'vue'
import useViewStore from '@/stores/ViewStore'
import useTestStore from '@/stores/TestStore'
import TestTranslate from '@/components/TestTranslate.vue'
import type { Ref } from 'vue'
import pastTestData from '@/views/HomeView/Lvl1/Mod1/GrammarTests/pastTestData.json'

const viewStore = useViewStore()
const testStore = useTestStore()

onBeforeRouteLeave((to) => {
    const toPath = to.path
    const previousPath = /^\/level-\d+\/module-\d+$/
    if (previousPath.test(toPath)) {
        viewStore.mainNavVisible = true
    }
})

const TEST_NAME = 'Test - Past'
const TEST_PARTS = 6

// importing component functions from ProgressHeader
const child = ref<InstanceType<typeof ProgressHeader> | null>(null)
const incrementProgress = () => child.value?.incrementProgress()

const currentPart: Ref<number> = ref(1)

const finalResult: Ref<number> = ref(0)

const testResultVisible: Ref<boolean> = ref(false)

function handleResult(isOver: boolean): void {
    if (isOver === true) {
        finalResult.value++
    }
    currentPart.value++

    if (currentPart.value > TEST_PARTS) {
        testResultVisible.value = true
    }
    incrementProgress()
}

function updateAllData() {
    testStore.updateStreak()
    testStore.testResults.tests['level-1'].modules['module-1'].lessons['test-past'].completed = true

    testStore.testResults.testsCompleted++
    testStore.testResults.answeredQuestions += TEST_PARTS
    testStore.testResults.correctQuestions += finalResult.value

    testStore.setTestResults()
}
</script>

<template>
    <div class="lesson-view-wrapper">
        <ProgressHeader :headerName="TEST_NAME" :totalParts="TEST_PARTS" ref="child" />
        <template v-for="item in pastTestData" :key="item.id">
            <TestTranslate
                v-if="item.id === currentPart"
                :toTranslate="item.question"
                :correctAnswer="item.answer"
                @isOver="handleResult"
            >
                <table class="four-column-table">
                    <tr>
                        <th>Vietniekvārds</th>
                        <th>Iet</th>
                        <th>Būt</th>
                        <th>Dot</th>
                    </tr>
                    <tr>
                        <td>es</td>
                        <td>gāju</td>
                        <td>biju</td>
                        <td>devu</td>
                    </tr>
                    <tr>
                        <td>tu</td>
                        <td>gāji</td>
                        <td>biji</td>
                        <td>devi</td>
                    </tr>
                    <tr>
                        <td>viņš, viņa</td>
                        <td>gāja</td>
                        <td>bija</td>
                        <td>deva</td>
                    </tr>
                    <tr>
                        <td>mēs</td>
                        <td>gājām</td>
                        <td>bijām</td>
                        <td>devām</td>
                    </tr>
                    <tr>
                        <td>jūs</td>
                        <td>gājāt</td>
                        <td>bijāt</td>
                        <td>devāt</td>
                    </tr>
                    <tr>
                        <td>viņi, viņas</td>
                        <td>gāja</td>
                        <td>bija</td>
                        <td>deva</td>
                    </tr>
                </table>
            </TestTranslate>
        </template>
        <div v-if="testResultVisible" class="final-score">
            <div class="final-score-content">
                <div class="test-results">
                    <h1>Test is over</h1>
                    <p>Your final score: {{ finalResult }}/{{ TEST_PARTS }}</p>
                </div>
                <RouterLink :to="{ name: 'module-1' }" tabindex="-1">
                    <button type="button" @click="updateAllData" class="btn-next">Return</button>
                </RouterLink>
            </div>
        </div>
    </div>
</template>

<style scoped>
.final-score {
    position: fixed;
    width: 100vw;
    height: 100vh;
    background-color: var(--black-a90);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 2;
    overflow: auto;
}

.final-score-content {
    background-color: var(--black-100);
    padding: 32px;
    border-radius: 16px;
    overflow: auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 32px;
    max-width: 90%;
    max-height: 90%;

    & h1,
    p {
        margin: 8px 0;
    }

}
</style>
