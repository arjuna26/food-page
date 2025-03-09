<template>
    <div class="relative w-full h-[200vh] text-white overflow-hidden bg-darker">
        <div class="absolute z-10 w-full mx-auto py-30 px-0 grid grid-cols-12 bg-darker">
            <div class="absolute opacity-50 right-24 flex flex-row gap-10 py-20">
                <a href="https://instagram.com/your-account"><img src="../icons/instagram.svg" alt="ig"
                        class="w-12"></a>
                <a href="https://instagram.com/your-account"><img src="../icons/soundcloud.svg" alt="ig"
                        class="w-12"></a>
            </div>
            <div class="col-span-4 flex flex-col text-left gap-10 ml-24">
                <h1 class="font-primary font-bold text-6xl text-light">
                    TITLE
                </h1>
                <div class="border-b border-b-mustard w-2/3"></div>
                <p class="font-primary italic font-semibold text-lighter text-2xl">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore
                    et dolore magna aliqua.
                </p>
            </div>
            <div class="col-span-6 m-24 relative w-full h-full">
                <div class="flex flex-col z-10 absolute top-[80%] translate-x-[180%]">
                    <h1 class="text-5xl font-primary font-bold" style="color: #EEEEEE">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #EFEBE8">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F0E8E2">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F0E5DC">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F1E2D6">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F1DFD0">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F2DCCA">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F2D9C4">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F3D6BE">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #F3D3B8">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #EEC9A9">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #EAC099">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #E6B683">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #E6AD6D">
                        HELLO WORLD</h1>
                </div>
                <img src="../icons/rectangle.svg" ref="expandingRect" alt=""
                    class="fixed min-w-screen object-cover transition-transform duration-1000 ease-out opacity-70">
                <img src="https://placehold.co/500x800" alt="Placeholder" class="absolute to transform">
            </div>
        </div>
    </div>
</template>

<script>
import { gsap } from 'gsap';

export default {
    name: 'Landing',
    data() {
        return {
            scrollTween: null,
            lastScrollY: 0,
            initialRectPosition: null
        }
    },
    mounted() {
        // Wait for DOM to be fully rendered
        this.$nextTick(() => {
            // Store initial rectangle position
            if (this.$refs.expandingRect) {
                // Keep the original position and styling as you had it
                const rect = this.$refs.expandingRect.getBoundingClientRect();
                this.initialRectPosition = {
                    top: rect.top,
                    left: rect.left,
                    width: rect.width,
                    height: rect.height
                };
            }

            // Set up smooth scroll tracking with GSAP
            this.setupScrollTracking();
        });
    },
    beforeUnmount() {
        // Clean up any GSAP animations
        if (this.scrollTween) {
            this.scrollTween.kill();
        }
        window.removeEventListener('scroll', this.handleScroll);
    },
    methods: {
        setupScrollTracking() {
            // Add scroll event listener for performance
            window.addEventListener('scroll', this.handleScroll);

            // Set up GSAP scroll-triggered animation
            this.scrollTween = gsap.to(this.$refs.expandingRect, {
                scale: 1, // Starting scale
                paused: true, // We'll control the playhead manually
                duration: 1, // This represents the full animation duration (not real-time)
                ease: "power2.out" // Smoother easing
            });

            // Set initial state
            gsap.set(this.$refs.expandingRect, {
                scale: 1, // Start at original size
                transformOrigin: "center center" // Expand from center
            });
        },
        handleScroll() {
            if (!this.$refs.expandingRect) return;

            // Get current scroll position
            const scrollY = window.scrollY;

            // Parameters for scaling
            const startScroll = 100;
            const endScroll = 3000; // Much higher value for slower expansion
            const maxScale = 15;

            // Calculate progress between 0 and 1
            let progress = 0;
            if (scrollY > startScroll) {
                progress = Math.min((scrollY - startScroll) / (endScroll - startScroll), 1);
            }

            // Apply scale using GSAP for smoother animation
            const targetScale = 1 + (progress * (maxScale - 1));
            gsap.to(this.$refs.expandingRect, {
                scale: targetScale,
                duration: 0.3, // Short duration for smoother but responsive animation
                overwrite: true, // Prevents animation queue build-up
                ease: "power2.out" // Smooth easing
            });

            this.lastScrollY = scrollY;
        }
    }
}
</script>