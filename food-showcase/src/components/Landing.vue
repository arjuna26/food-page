<template>
    <div class="relative w-full text-white overflow-hidden bg-darker" :style="{ height: `${detailItems.length * 100}vh` }">
        <!-- Fixed header elements -->
        <img src="../icons/black-rectangle.svg" ref="blackRect" alt="" 
        class="absolute origin-center">
        <div class="absolute top-0 w-full z-30 py-10">
            <div class="absolute left-24 flex flex-row gap-10 placeholder-yellow-200 opacity-50 top-[50px]">
                <a href="https://instagram.com/your-account"><img src="../icons/instagram.svg" alt="ig" class="w-12"></a>
                <a href="https://instagram.com/your-account"><img src="../icons/soundcloud.svg" alt="ig" class="w-12"></a>
            </div>
        </div>

        <!-- Animated background shapes -->
        <div class="fixed inset-0 z-0 overflow-hidden">
            <!-- Main rectangle -->
            <img src="../icons/large-rectangle.svg" ref="parallaxRect" alt="" 
                class="absolute origin-center">
                
            <!-- Black rectangle -->
            <img src="../icons/black-rectangle.svg" ref="blackRect" alt="" 
                class="absolute origin-center">
        </div>

        <!-- Vertical brand text -->
        <div ref="brandText" class="fixed right-[150px] h-screen  z-20  -rotate-180 text-mustard opacity-20">
            <div class="animate-scroll-up">
                <p class="vertical-text text-8xl font-primary tracking-widest ">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest ">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest ">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
            </div>
        </div>

        <!-- Right vertical brand text -->
        <div ref="brandTextRight" class="fixed right-[50px] h-screen z-20 text-mustard opacity-20">
            <div class="animate-scroll-down">
                <p class="vertical-text text-8xl font-primary tracking-widest pb-8">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest pb-8">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest pb-8">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
                <p class="vertical-text text-8xl font-primary tracking-widest">BRAND NAME</p>
            </div>
        </div>

        <!-- Main content container -->
        <div class="fixed inset-0 z-10 grid grid-cols-12 h-screen">
            <!-- Left column with title and content -->
            <div class="col-span-4 flex flex-col text-left gap-10 ml-24 mt-32 relative">
                
                <!-- Scroll progress indicator that appears when title slides away -->
                <div ref="scrollProgress" class="absolute top-0 left-0 w-full opacity-0 transition-all duration-700">
                    <div class="flex flex-col gap-4">
                        <div class="flex justify-between items-center">
                            <span class="text-sm text-light font-primary"></span>
                            <span class="text-sm text-mustard">{{ activeIndex + 1 }} / {{ detailItems.length }}</span>
                        </div>
                        <div class="h-2 bg-gray-800 rounded-full w-full overflow-hidden">
                            <div 
                                class="h-full bg-mustard rounded-full transition-all duration-500 ease-out"
                                :style="{ width: `${(activeIndex / (detailItems.length - 1)) * 100}%` }"
                            ></div>
                        </div>
                        <div class="flex justify-between text-xs text-gray-500">
                            <span></span>
                            <span></span>
                        </div>
                    </div>
                </div>
                
                <!-- Description for the active detail item -->
                <div class="mt-8">
                    <div v-for="(item, index) in detailItems" 
                         :key="index"
                         :ref="`detailDescription${index}`"
                         class="detail-description absolute w-full transition-opacity duration-500"
                         :class="activeIndex === index ? 'opacity-100' : 'opacity-0'">
                        <h3 class="font-primary font-semibold text-3xl text-light mb-3">
                            {{ item.text }}
                        </h3>
                        <p class="font-primary text-light text-2xl leading-relaxed">
                            {{ item.description }}
                        </p>
                    </div>
                </div>
            </div>

            <!-- Right column with images and navigation -->
            <div class="col-span-8 relative h-full">
                <!-- Up arrow for navigation -->
                <button 
                    @click="navigateToSection('prev')" 
                    class="absolute top-[100px] left-[100px] w-[500px] h-[40px] flex items-center justify-center text-mustard hover:text-white transition-colors duration-300 z-30"
                    :class="{ 'opacity-50 cursor-not-allowed': activeIndex === 0 }"
                    :disabled="activeIndex === 0"
                >
                    <svg viewBox="0 0 24 14" class="w-full h-full">
                        <path 
                            d="M 2 12 L 12 2 L 22 12" 
                            fill="none" 
                            stroke="currentColor" 
                            stroke-width="3" 
                            stroke-linecap="round"
                            stroke-linejoin="round"
                        />
                    </svg>
                </button>

                <!-- Image container -->
                <div class="image-container absolute left-[100px] top-[200px] w-[500px] h-[500px] overflow-hidden z-20">
                    <img 
                        v-for="(item, index) in detailItems" 
                        :key="index"
                        :src="item.image"
                        :ref="`detailImage${index}`"
                        :class="`detail-image absolute w-full h-full object-cover transition-opacity duration-500 ${activeIndex === index ? 'opacity-100' : 'opacity-0'}`"
                        alt="Detail image">
                </div>
                
                <!-- Down arrow for navigation -->
                <button 
                    @click="navigateToSection('next')" 
                    class="absolute top-[760px] left-[100px] w-[500px] h-[40px] flex items-center justify-center text-mustard hover:text-white transition-colors duration-300 z-30"
                    :class="{ 'opacity-50 cursor-not-allowed': activeIndex === detailItems.length - 1 }"
                    :disabled="activeIndex === detailItems.length - 1"
                >
                    <svg viewBox="0 0 24 14" class="w-full h-full">
                        <path 
                            d="M 2 2 L 12 12 L 22 2" 
                            fill="none" 
                            stroke="currentColor" 
                            stroke-width="3" 
                            stroke-linecap="round"
                            stroke-linejoin="round"
                        />
                    </svg>
                </button>
            </div>
        </div>

        <!-- Invisible scroll sections -->
        <div class="relative z-0">
            <div v-for="(_, index) in detailItems" 
                :key="index" 
                :ref="`section${index}`"
                class="h-screen w-full">
            </div>
        </div>

        <!-- Credits row -->
        <div class="fixed bottom-8 left-24 z-30 flex flex-col gap-2">
            <div class="flex gap-6">
                <a href="#" class="font-primary text-xs text-light hover:text-mustard transition-colors">Privacy Policy</a>
                <a href="#" class="font-primary text-xs text-light hover:text-mustard transition-colors">Terms of Service</a>
                <a href="#" class="font-primary text-xs text-light hover:text-mustard transition-colors">back to 1/9</a>
            </div>
        </div>
    </div>
</template>

<script>
import { gsap } from 'gsap';

export default {
    data() {
        return {
            activeIndex: 0,
            lastScrollY: 0,
            // Add a state tracker for the title to prevent unwanted animations
            titleState: 'visible', // 'visible' or 'hidden'
            detailItems: [  // change these to product details, or if one product then different images of the product
                {
                    text: 'Welcome',
                    image: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c',
                    description: 'How are you doing today?'
                },
                {
                    text: 'DETAILS 2',
                    image: 'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38',
                    description: 'Freshly baked pizza with a perfect golden crust, topped with melted cheese, fresh basil, and premium ingredients. A classic favorite with our special homemade sauce.'
                },
                {
                    text: 'DETAILS 3',
                    image: 'https://images.unsplash.com/photo-1565958011703-44f9829ba187',
                    description: 'Artisanal pasta dish featuring al dente noodles, fresh seasonal vegetables, and a light sauce that highlights the natural flavors. Garnished with fresh herbs and parmesan.'
                },
                {
                    text: 'DETAILS 4',
                    image: 'https://images.unsplash.com/photo-1482049016688-2d3e1b311543',
                    description: 'A rustic breakfast spread with freshly baked bread, farm-fresh eggs, and seasonal fruits. Start your day with this wholesome and nourishing selection of morning favorites.'
                },
                {
                    text: 'DETAILS 5',
                    image: 'https://images.unsplash.com/photo-1467003909585-2f8a72700288',
                    description: 'Seasonal fruit platter featuring a colorful array of berries, citrus, and tropical fruits. A refreshing and healthy option that is perfect for sharing or as a light dessert.'
                },
                {
                    text: 'DETAILS 6',
                    image: 'https://images.unsplash.com/photo-1484723091739-30a097e8f929',
                    description: 'Gourmet coffee experience with freshly ground beans, creating a rich and aromatic cup with perfect crema. Paired with a sweet treat for the ideal afternoon pick-me-up.'
                },
                {
                    text: 'DETAILS 7',
                    image: 'https://images.unsplash.com/photo-1540189549336-e6e99c3679fe',
                    description: 'A beautifully arranged food bowl with grilled protein, fresh vegetables, and a variety of textures and flavors. A balanced meal that is both nutritious and satisfying.'
                },
                {
                    text: 'DETAILS 8',
                    image: 'https://images.unsplash.com/photo-1567620905732-2d1ec7ab7445',
                    description: 'Decadent chocolate cake with layers of rich ganache and a velvety smooth texture. The perfect indulgence for chocolate lovers and special celebrations.'
                },
                {
                    text: 'DETAILS 9',
                    image: 'https://images.unsplash.com/photo-1565299507177-b0ac66763828',
                    description: 'Handcrafted cocktail made with premium spirits, fresh juices, and artisanal ingredients. A sophisticated beverage with complex flavors and beautiful presentation.'
                }
            ],
            sectionPositions: [],
            rectInitialSize: {
                width: 600,
                height: 600
            },
            blackRectInitialSize: {
                width: 1600,
                height: 1000
            },
            // Add throttle timer for scroll events
            scrollThrottleTimer: null,
            // Cache document height to avoid recalculating it on every scroll
            docHeight: 0,
            // Cache window height
            windowHeight: 0,
            // Store animation timelines for reuse
            animations: {
                title: null,
                progress: null
            },
            // Flag to prevent scroll handling during programmatic scrolling
            isNavigating: false
        }
    },
    mounted() {
        // Initialize all shapes at once with a single batch update
        this.initializeElements();
        
        // Cache window height
        this.windowHeight = window.innerHeight;
        
        // Add resize listener to update cached values
        window.addEventListener('resize', this.handleResize);
        
        // Wait for DOM to be fully rendered AND images to load
        window.addEventListener('load', this.calculateSectionPositions);
        
        // Set up throttled scroll event listener
        window.addEventListener('scroll', this.throttledScrollHandler);
        
        // Fallback in case images are already loaded
        if (document.readyState === 'complete') {
            this.calculateSectionPositions();
        }
        
        // Force initial scroll to top
        window.scrollTo(0, 0);
    },
    beforeUnmount() {
        // Clean up all event listeners
        window.removeEventListener('scroll', this.throttledScrollHandler);
        window.removeEventListener('load', this.calculateSectionPositions);
        window.removeEventListener('resize', this.handleResize);
        
        // Clear any pending timers
        if (this.scrollThrottleTimer) {
            clearTimeout(this.scrollThrottleTimer);
        }
    },
    methods: {
        // Combine initialization methods into a single method for better performance
        initializeElements() {
            // Initialize rectangles
            if (this.$refs.parallaxRect) {
                gsap.set(this.$refs.parallaxRect, {
                    width: this.rectInitialSize.width,
                    height: this.rectInitialSize.height,
                    left: '50%',
                    top: '50%',
                    xPercent: -50,
                    yPercent: -50,
                    rotation: 0,
                    opacity: 0.2
                });
            }
            
            if (this.$refs.blackRect) {
                gsap.set(this.$refs.blackRect, {
                    width: this.blackRectInitialSize.width,
                    height: this.blackRectInitialSize.height,
                    left: '900px',
                    top: '200px',
                    zIndex: 5
                });
            }
            
            // Initialize descriptions
            this.detailItems.forEach((_, index) => {
                const descRef = this.$refs[`detailDescription${index}`];
                if (descRef && descRef[0]) {
                    gsap.set(descRef[0], {
                        opacity: index === 0 ? 1 : 0,
                        position: 'absolute',
                        width: '100%'
                    });
                }
            });
            
            // Pre-create animation timelines for reuse
            this.createAnimationTimelines();
        },
        
        // Create reusable animation timelines
        createAnimationTimelines() {
            // Make sure refs exist before creating animations
            if (!this.$refs.titleContent || !this.$refs.scrollProgress) return;
            
            // Create title animations
            this.animations.title = {
                show: gsap.timeline({ paused: true })
                    .to(this.$refs.titleContent, {
                        x: 0,
                        opacity: 1,
                        duration: 0.7,
                        ease: "power2.out"
                    }),
                hide: gsap.timeline({ paused: true })
                    .to(this.$refs.titleContent, {
                        x: -100,
                        opacity: 0,
                        duration: 0.7,
                        ease: "power2.in"
                    })
            };
            
            // Create progress animations
            this.animations.progress = {
                show: gsap.timeline({ paused: true })
                    .to(this.$refs.scrollProgress, {
                        opacity: 1,
                        y: 0,
                        duration: 0.7,
                        ease: "power2.out"
                    }),
                hide: gsap.timeline({ paused: true })
                    .to(this.$refs.scrollProgress, {
                        opacity: 0,
                        y: 20,
                        duration: 0.5,
                        ease: "power2.in"
                    })
            };
            
            // Set initial states for the elements
            gsap.set(this.$refs.titleContent, { 
                x: 0,
                opacity: 1
            });
            
            gsap.set(this.$refs.scrollProgress, {
                opacity: 0,
                y: 20
            });
            
            // Initialize title state based on active index
            this.titleState = this.activeIndex === 0 ? 'visible' : 'hidden';
        },
        
        // Handle window resize
        handleResize() {
            // Update cached values
            this.windowHeight = window.innerHeight;
            this.docHeight = document.body.scrollHeight - this.windowHeight;
            
            // Recalculate section positions
            this.calculateSectionPositions();
        },
        
        // Throttle scroll handler for better performance
        throttledScrollHandler() {
            // Skip if a throttle is already in progress
            if (this.scrollThrottleTimer) return;
            
            // Use requestAnimationFrame for smoother performance
            this.scrollThrottleTimer = requestAnimationFrame(() => {
                this.handleScroll();
                this.scrollThrottleTimer = null;
            });
        },
        
        calculateSectionPositions() {
            // Calculate the position of each section for scroll detection
            this.sectionPositions = [];
            
            // Use a more efficient loop
            const sections = [];
            for (let i = 0; i < this.detailItems.length; i++) {
                const sectionRef = this.$refs[`section${i}`];
                if (sectionRef && sectionRef[0]) {
                    sections.push(sectionRef[0]);
                }
            }
            
            // Batch DOM reads to avoid layout thrashing
            sections.forEach(section => {
                const rect = section.getBoundingClientRect();
                this.sectionPositions.push(rect.top + window.scrollY);
            });
            
            // Cache document height
            this.docHeight = document.body.scrollHeight - this.windowHeight;
            
            // Initial check for active section
            this.handleScroll();
        },
        
        handleScroll() {
            // Skip scroll handling if we're currently navigating programmatically
            if (this.isNavigating) return;
            
            const scrollY = window.scrollY;
            const scrollMid = scrollY + (this.windowHeight / 2);
            
            // Use binary search for more efficient section finding
            let newActiveIndex = this.findActiveSection(scrollMid);
            
            // Update active index if changed
            if (newActiveIndex !== this.activeIndex) {
                // Batch animations together
                this.updateActiveSection(this.activeIndex, newActiveIndex);
                this.activeIndex = newActiveIndex;
            }
            
            // Only update parallax effects if scroll position changed significantly
            if (Math.abs(scrollY - this.lastScrollY) > 5) {
                // Calculate once and reuse
                const scrollProgress = this.docHeight ? scrollY / this.docHeight : 0;
                this.updateParallaxEffects(scrollProgress);
                this.lastScrollY = scrollY;
            }
        },
        
        // Binary search for finding active section - much more efficient for large lists
        findActiveSection(scrollMid) {
            // For small lists, linear search is fine
            if (this.sectionPositions.length < 10) {
                for (let i = this.sectionPositions.length - 1; i >= 0; i--) {
                    if (scrollMid >= this.sectionPositions[i]) {
                        return i;
                    }
                }
                return 0;
            }
            
            // For larger lists, use binary search
            let low = 0;
            let high = this.sectionPositions.length - 1;
            
            while (low <= high) {
                const mid = Math.floor((low + high) / 2);
                
                if (scrollMid < this.sectionPositions[mid]) {
                    high = mid - 1;
                } else if (mid < this.sectionPositions.length - 1 && 
                          scrollMid >= this.sectionPositions[mid + 1]) {
                    low = mid + 1;
                } else {
                    return mid;
                }
            }
            
            return 0;
        },
        
        // Batch update all animations related to changing active section
        updateActiveSection(oldIndex, newIndex) {
            // Animate text highlights
            this.animateTextHighlight(oldIndex, newIndex);
            
            // Animate descriptions
            this.animateDescriptionTransition(oldIndex, newIndex);
            
            // Handle title visibility with state tracking
            this.handleTitleVisibility(newIndex);
        },
        
        // Update all parallax effects with a single progress value
        updateParallaxEffects(scrollProgress) {
            // Update main rectangle
            if (this.$refs.parallaxRect) {
                const scaleAmount = 1 + (scrollProgress * 5);
                const opacityValue = 0.2 + (scrollProgress * 0.6);
                
                gsap.to(this.$refs.parallaxRect, {
                    scale: scaleAmount,
                    opacity: opacityValue,
                    duration: 0.5,
                    ease: "power1.out"
                });
            }
            
            // Update black rectangle
            if (this.$refs.blackRect) {
                const scaleAmount = 1 + Math.min(1, scrollProgress * 3);
                
                gsap.to(this.$refs.blackRect, {
                    scale: scaleAmount,
                    duration: 0.5,
                    ease: "power1.out"
                });
            }

            // Update brand text positions
            if (this.$refs.brandText) {
                const yOffset = scrollProgress * 100;
                
                gsap.to(this.$refs.brandText, {
                    y: `${yOffset - 50}%`,
                    duration: 0.5,
                    ease: "power1.out"
                });
            }

            // Update right brand text with opposite movement
            if (this.$refs.brandTextRight) {
                const yOffset = scrollProgress * -100; // Negative value for opposite direction
                
                gsap.to(this.$refs.brandTextRight, {
                    y: `${yOffset - 50}%`,
                    duration: 0.5,
                    ease: "power1.out"
                });
            }
        },
        
        animateDescriptionTransition(oldIndex, newIndex) {
            // Get both refs at once to batch DOM access
            const oldDescRef = this.$refs[`detailDescription${oldIndex}`];
            const newDescRef = this.$refs[`detailDescription${newIndex}`];
            
            // Batch animations together
            if (oldDescRef && oldDescRef[0]) {
                gsap.to(oldDescRef[0], {
                    opacity: 0,
                    duration: 0.5,
                    ease: 'power2.out'
                });
            }
            
            if (newDescRef && newDescRef[0]) {
                gsap.to(newDescRef[0], {
                    opacity: 1,
                    duration: 0.5,
                    delay: 0.1,
                    ease: 'power2.in'
                });
            }
        },
        
        handleTitleVisibility(activeIndex) {
            if (!this.$refs.titleContent || !this.$refs.scrollProgress || !this.animations.title || !this.animations.progress) return;
            
            // Use state tracking to prevent unwanted animations
            if (activeIndex === 0) {
                // Only animate if title is currently hidden
                if (this.titleState === 'hidden') {
                    this.animations.title.show.restart();
                    this.animations.progress.hide.restart();
                    this.titleState = 'visible';
                }
            } else {
                // Only animate if title is currently visible and we're moving away from first section
                if (this.titleState === 'visible') {
                    this.animations.title.hide.restart();
                    this.animations.progress.show.restart();
                    this.titleState = 'hidden';
                }
            }
        },
        
        animateTextHighlight(oldIndex, newIndex) {
            const textElements = this.$refs.detailTexts;
            if (!textElements) return;
            
            // Batch animations together
            const animations = [];
            
            if (textElements[oldIndex]) {
                animations.push(
                    gsap.to(textElements[oldIndex], {
                        color: '#3A3C3E',
                        duration: 0.3,
                        ease: 'power1.out'
                    })
                );
            }
            
            if (textElements[newIndex]) {
                animations.push(
                    gsap.to(textElements[newIndex], {
                        color: '#EEEEEE',
                        duration: 0.3,
                        ease: 'power1.out'
                    })
                );
            }
        },
        
        // New method to handle navigation via arrows or clicking on details
        navigateToSection(target) {
            let newIndex;
            
            // Handle different types of navigation targets
            if (target === 'next') {
                newIndex = Math.min(this.activeIndex + 1, this.detailItems.length - 1);
            } else if (target === 'prev') {
                newIndex = Math.max(this.activeIndex - 1, 0);
            } else if (typeof target === 'number') {
                newIndex = target;
            } else {
                return; // Invalid target
            }
            
            // Don't do anything if we're already at the target index
            if (newIndex === this.activeIndex) return;
            
            // Set flag to prevent scroll handler from interfering
            this.isNavigating = true;
            
            // Get the target section element
            const targetSection = this.$refs[`section${newIndex}`];
            if (targetSection && targetSection[0]) {
                // Calculate the progress for parallax effects based on target section
                const scrollProgress = newIndex / (this.detailItems.length - 1);
                
                // Update parallax effects immediately
                this.updateParallaxEffects(scrollProgress);
                
                // Scroll to the target section
                window.scrollTo({
                    top: this.sectionPositions[newIndex],
                    behavior: 'smooth'
                });
                
                // Update active index and trigger animations
                this.updateActiveSection(this.activeIndex, newIndex);
                this.activeIndex = newIndex;
                
                // Reset navigation flag after animation completes
                setTimeout(() => {
                    this.isNavigating = false;
                }, 1000); // Slightly longer than the scroll animation
            }
        }
    }
}
</script>

<style scoped>
.detail-image {
    transition: opacity 0.5s ease-in-out;
}

.detail-description {
    transition: opacity 0.5s ease-in-out;
    position: relative;
}

/* Ensure the page has enough height for scrolling */
.h-screen {
    height: 100vh;
}

/* Add some padding at the bottom to ensure we can scroll to the last section fully */
.relative.z-0 {
    padding-bottom: 20vh;
}

/* Add hover effect to detail items */
.cursor-pointer:hover {
    transform: translateX(5px);
    transition: transform 0.3s ease;
}

/* Style for navigation arrows */
button:not(:disabled):hover {
    transform: scaleY(1.2);
}

button:not(:disabled):hover path {
    stroke-width: 4;
}

button {
    transition: all 0.3s ease;
}

button path {
    transition: all 0.3s ease;
}

/* Vertical text styling */
.vertical-text {
    writing-mode: vertical-rl;
    text-orientation: mixed;
}

@keyframes scrollUp {
    from {
        transform: translateY(0%);
    }
    to {
        transform: translateY(-50%);
    }
}

@keyframes scrollDown {
    from {
        transform: translateY(-50%);
    }
    to {
        transform: translateY(0%);
    }
}

.animate-scroll-up {
    animation: scrollUp 20s linear infinite;
}

.animate-scroll-down {
    animation: scrollDown 20s linear infinite;
}
</style>