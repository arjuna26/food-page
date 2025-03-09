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
                <div class="flex flex-col z-10 absolute top-[120%] translate-x-[180%]">
                    <h1 class="text-5xl font-primary font-bold" style="color: #EEEEEE">
                    </h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #DADADA">
                        4 USB Ports</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #C6C7C8">
                        2.8 x 1.97</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #B2B3B4">
                        TA-105, Grey</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #9EA0A1">
                        certified RoHS</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #8A8C8D">
                        220 volts</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #76787A">
                        489 5★ reviews</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #626466">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #4E5052">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #3A3C3E">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #393A3B">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #383A3B">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #3C3D3E">
                        HELLO WORLD</h1>
                    <h1 class="text-5xl font-primary font-bold" style="color: #3A3B3C">
                        HELLO WORLD</h1>
                </div>
                <img src="../icons/rectangle.svg" ref="expandingRect" alt=""
                    class="fixed min-w-screen transition-transform duration-1000 ease-out opacity-70">
                <img src="https://images.squarespace-cdn.com/content/v1/5031e8d184ae7fae2e67d0ad/1582936781964-KUFG8CDVVDNY8WSEYAIA/Black-White-Still-Life-Evi-Abeler-Photographer.jpg"
                    alt="Placeholder" class="absolute to transform w-[500px] h-[800px] ">
            </div>
        </div>
    </div>
</template>

<script>
import { gsap } from 'gsap';

export default {
    data() {
        return {
            initialRectPosition: null,
            lastScrollY: 0,
            scalingRect: null
        }
    },
    mounted() {
        // Wait for DOM to be fully rendered AND images to load
        window.addEventListener('load', this.initializeRect);

        // Fallback in case images are already loaded
        if (document.readyState === 'complete') {
            this.initializeRect();
        }
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.handleScroll);
        window.removeEventListener('load', this.initializeRect);
        // Clean up the clone if it exists
        if (this.scalingRect && document.body.contains(this.scalingRect)) {
            document.body.removeChild(this.scalingRect);
        }
    },
    methods: {
        initializeRect() {
            if (this.$refs.expandingRect) {
                // Hide the original rectangle immediately to prevent flash
                this.$refs.expandingRect.style.opacity = '0';
                
                // Reset scroll position
                window.scrollTo(0, 0);

                // After a brief delay to ensure everything is settled
                setTimeout(() => {
                    // Get the exact position and dimensions after everything is loaded
                    const rect = this.$refs.expandingRect.getBoundingClientRect();

                    // Store the original values
                    this.initialRectPosition = {
                        top: rect.top,
                        left: rect.left,
                        width: rect.width,
                        height: rect.height
                    };

                    // Clone the rectangle to a new element that will be fixed and scaled
                    const clone = this.$refs.expandingRect.cloneNode(true);
                    clone.id = 'scalingRect';

                    // First, add it to DOM with visibility:hidden for proper positioning
                    clone.style.visibility = 'hidden';
                    document.body.appendChild(clone);

                    // Then set all styles - using px for top instead of % which was causing the issue
                    clone.style.position = 'fixed';
                    clone.style.top = `${rect.top}px`;  // Fixed: was using % instead of px
                    clone.style.left = `${rect.left}px`;
                    clone.style.width = `${rect.width}px`;
                    clone.style.height = `${rect.height}px`;
                    clone.style.transformOrigin = 'center center';
                    clone.style.zIndex = '5';
                    clone.style.opacity = '0.7';
                    clone.style.visibility = 'visible';
                    clone.style.transition = 'none';

                    // Store reference to the clone
                    this.scalingRect = clone;

                    // Set up scroll tracking
                    this.setupScrollTracking();
                }, 100);
            }
        },
        setupScrollTracking() {
            // Add scroll event listener
            window.addEventListener('scroll', this.handleScroll);

            // Initialize at current scroll position
            this.handleScroll();
        },
        handleScroll() {
            if (!this.scalingRect || !this.initialRectPosition) return;

            // Get current scroll position
            const scrollY = window.scrollY;

            // Parameters for scaling
            const startScroll = 0;
            const endScroll = 2000;
            const maxScale = 5;

            // Calculate progress between 0 and 1
            let progress = 0;
            if (scrollY > startScroll) {
                progress = Math.min((scrollY - startScroll) / (endScroll - startScroll), 1);
            }

            // Calculate target scale
            const targetScale = 1 + (progress * (maxScale - 1));

            // Explicitly set the position each time to prevent drift
            this.scalingRect.style.top = `${this.initialRectPosition.top}px`;
            this.scalingRect.style.left = `${this.initialRectPosition.left}px`;

            // Apply scale using GSAP to the clone
            gsap.to(this.scalingRect, {
                scale: targetScale,
                duration: 0.3,
                overwrite: true,
                ease: "power2.out"
            });

            this.lastScrollY = scrollY;
        }
    }
}
</script>