<template>
    <div class="d-xl-none d-block" style="height:500px;">

    </div>
    <div class="container my-4" style="max-width:1400px;width:100%;">
        <div class="row g-4">
            <div class="col-xl-3 col-12">
                <div class="p-3 rounded-4 glass">
                    <h5 class="m-0">Current time</h5>
                    <div class="mt-3">
                        <div class="bg-dark text-white rounded-4 border text-center p-2">
                            <div style="width:2.7rem;height:2.7rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <h1 v-if="!props.about.loading" class="m-0 p-0">
                                {{ pad(props.about?.current_time?.getHours() ?? 0) }}:{{ pad(props.about?.current_time?.getMinutes() ?? 0) }}
                            </h1>
                        </div>
                        <div class="m-0 mt-3 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-calendar2 text-secondary"></i>
                            Date:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props.about?.current_time?.toLocaleDateString() || "00" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-compass text-secondary"></i>
                            Timezone:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.timezone?.id || "Unknown" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-clock-history text-secondary"></i>
                            UTC:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">UTC{{ props?.about?.timezone?.utc || "Unknown" }}</b>
                        </div>
                    </div>
                </div>
                <div class="p-3 rounded-4 glass mt-4">
                    <h5 class="m-0">About me</h5>
                    <div class="mt-3">
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-wifi text-secondary"></i>
                            IP:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.ip || "Unknown" }}</b>
                        </div>
                        <div v-if="device?.client?.type" class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-laptop text-secondary"></i>
                            Client:
                            <b class="ms-auto">{{ device?.client?.type || "Unknown" }}</b>
                        </div>
                        <div v-if="device?.device?.type" class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-pc-display text-secondary"></i>
                            Device:
                            <b class="ms-auto">{{ device?.device?.type || "Unknown" }}</b>
                        </div>
                        <div v-if="device?.os?.name" class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex align-items-center gap-2">
                            <i class="bi bi-motherboard text-secondary"></i>
                            OS:
                            <b class="ms-auto text-end">{{ device?.os?.name || "Unknown" }} {{ device?.os?.version || "" }} {{ device?.os?.platform || "" }}</b>
                        </div>
                    </div>
                    
                </div>
            </div>
            <div class="col-xl-6 d-xl-block d-none"></div>
            <div class="col-xl-3 col-12">
                <div class="p-3 rounded-4 glass">
                    <h5 class="m-0">
                        <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border" role="status">
                            <span class="visually-hidden">Loading...</span>
                        </div>
                        <span v-if="!props.about.loading">
                            {{ props.about?.city || "Unknown" }},
                            {{ props.about?.country || "Unknown" }}
                        </span>
                    </h5>
                    <div class="mt-3">
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-globe-central-south-asia text-secondary"></i>
                            Continent:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.continent || "Unknown" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-geo-alt text-secondary"></i>
                            Country:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.country || "Unknown" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-star text-secondary"></i>
                            Capital:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.capital || "Unknown" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-compass text-secondary"></i>
                            Latitude:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.lat || "Unknown" }}</b></div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-compass text-secondary"></i>
                            Longitude: 
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.lon || "Unknown" }}</b>
                        </div>
                    </div>
                    
                </div>
                <div class="p-3 rounded-4 glass mt-4">
                    <h5 class="m-0">Your Internet Connection</h5>
                    <div class="mt-3">
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex align-items-center gap-2">
                            <i class="bi bi-router text-secondary"></i>
                            Provider:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b v-if="!props.about.loading" class="ms-auto text-end">{{ props?.about?.connection?.org || "Unknown" }}</b>
                        </div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-globe text-secondary"></i>
                            ASN:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.connection?.asn || "Unknown" }}</b></div>
                        <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
                            <i class="bi bi-star text-secondary"></i>
                            Domain:
                            <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border ms-auto" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <b class="ms-auto" v-if="!props.about.loading">{{ props?.about?.connection?.domain || "Unknown" }}</b>
                        </div>
                    </div>
                    
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .glass {
        background: rgba(255, 255, 255, 0.8);
        border-radius: 16px;
        box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        backdrop-filter: blur(5px);
        -webkit-backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.3);
    }
</style>

<script setup>
    import { watchEffect } from "vue";
    const props = defineProps({
        about:Object
    })
    import DeviceDetector from "device-detector-js";
    const detector = new DeviceDetector();
    const device = detector.parse(navigator.userAgent);
    const pad = (n) => String(n).padStart(2, "0");
    watchEffect(()=>{
        console.log(props.about)
    })
</script>