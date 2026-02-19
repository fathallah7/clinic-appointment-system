<script setup>
import { iconPath } from '@/utils/icons';

const props = defineProps({
    show: Boolean,
    myApt: Object
})

const emit = defineEmits(['close', 'cancel'])

const fmt = (d) => new Date(d).toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric' })
</script>

<template>
    <teleport to="body">
        <transition name="modal">
            <div v-if="show" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-md"
                @click.self="emit('close')">
                <div class="bg-white rounded-3xl w-full max-w-lg p-8 shadow-2xl">
                    <div class="flex justify-between items-start mb-8">
                        <div>
                            <h3 class="text-3xl font-bold text-gray-900">My Appointment</h3>
                            <p class="text-gray-500 mt-1">Manage your booking</p>
                        </div>
                        <button @click="emit('close')"
                            class="w-12 h-12 hover:bg-gray-100 rounded-xl flex items-center justify-center transition group">
                            <svg class="w-6 h-6 text-gray-400 group-hover:text-gray-600 transition" fill="none"
                                stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    :d="iconPath.close" />
                            </svg>
                        </button>
                    </div>

                    <div v-if="!myApt" class="py-16 text-center">
                        <div class="w-24 h-24 bg-gray-100 rounded-3xl flex items-center justify-center mx-auto mb-6">
                            <svg class="w-12 h-12 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    :d="iconPath.calendar" />
                            </svg>
                        </div>
                        <p class="text-gray-600 font-semibold text-lg mb-6">No appointment scheduled</p>
                        <a href="#appointments" @click="emit('close')"
                            class="inline-flex items-center gap-2 px-6 py-3 bg-gradient-to-r from-emerald-600 to-teal-600 text-white font-semibold rounded-xl hover:from-emerald-700 hover:to-teal-700 transition-all shadow-lg shadow-emerald-500/30">
                            Book Now
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    :d="iconPath.arrow" />
                            </svg>
                        </a>
                    </div>

                    <div v-else class="space-y-6">
                        <div
                            class="bg-gradient-to-br from-emerald-50 to-teal-50 rounded-2xl p-8 border-2 border-emerald-200">
                            <div class="flex items-center gap-6 mb-6">
                                <div
                                    class="w-20 h-20 bg-gradient-to-br from-emerald-600 to-teal-600 rounded-2xl flex items-center justify-center shadow-xl shadow-emerald-500/30">
                                    <svg class="w-10 h-10 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            :d="iconPath.calendar" />
                                    </svg>
                                </div>
                                <div>
                                    <p class="text-4xl font-bold text-gray-900">{{ myApt.slot.start_time }}</p>
                                    <p class="text-gray-600 font-medium mt-1">{{ fmt(myApt.slot.date) }}</p>
                                </div>
                            </div>
                            <div class="inline-flex items-center gap-2 px-4 py-2 rounded-xl font-semibold"
                                :class="myApt.status === 'confirmed' ? 'bg-emerald-200 text-emerald-800' : 'bg-red-200 text-red-800'">
                                <svg v-if="myApt.status === 'confirmed'" class="w-5 h-5" fill="currentColor"
                                    viewBox="0 0 20 20">
                                    <path fill-rule="evenodd" :d="iconPath.check" clip-rule="evenodd" />
                                </svg>
                                {{ myApt.status === 'confirmed' ? 'Confirmed' : myApt.status }}
                            </div>
                        </div>
                        <button @click="emit('cancel')"
                            class="w-full py-4 border-2 border-gray-300 text-gray-700 font-semibold rounded-xl hover:bg-red-50 hover:border-red-300 hover:text-red-700 transition-all">Cancel
                            Appointment</button>
                    </div>
                </div>
            </div>
        </transition>
    </teleport>
</template>

<style scoped>
.modal-enter-active,
.modal-leave-active {
    transition: all 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
    opacity: 0;
}

.modal-enter-from>div,
.modal-leave-to>div {
    transform: scale(0.95) translateY(20px);
}
</style>
