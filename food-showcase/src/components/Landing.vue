<template>
    <div class="relative w-full text-white overflow-hidden bg-darker" :style="{ height: `${detailItems.length * 100}vh` }">
        <!-- Fixed header elements -->
        <div class="absolute top-0 w-full z-30 py-10">
            <div class="absolute right-24 flex flex-row gap-10 placeholder-yellow-200 opacity-50 top-[50px]">
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

        <!-- Main content container -->
        <div class="fixed inset-0 z-10 grid grid-cols-12 h-screen">
            <!-- Left column with title and content -->
            <div class="col-span-4 flex flex-col text-left gap-10 ml-24 mt-24 relative">
                <!-- Title content wrapper for animation -->
                <div ref="titleContent" class="transition-all duration-700">
                    <h1 class="font-primary font-bold text-6xl text-light mb-10">
                        TITLE
                    </h1>
                    <div class="border-b border-b-mustard w-2/3 mb-10"></div>
                </div>
                
                <!-- Scroll progress indicator that appears when title slides away -->
                <div ref="scrollProgress" class="absolute top-0 left-0 w-full opacity-0 transition-all duration-700">
                    <div class="flex flex-col gap-4">
                        <div class="flex justify-between items-center">
                            <span class="text-sm text-light font-primary">progress</span>
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
                    <h3 class="font-primary font-semibold text-3xl text-light mb-3">
                        {{ detailItems[activeIndex].text }}
                    </h3>
                    <p class="font-primary text-light text-2xl leading-relaxed">
                        {{ detailItems[activeIndex].description }}
                    </p>
                </div>
            </div>

            <!-- Right column with DETAILS and images -->
            <div class="col-span-8 relative h-full">
                <!-- DETAILS text column -->
                <div class="flex flex-col z-10 absolute top-[200px] right-[200px]">
                    <div v-for="(item, index) in detailItems" 
                        :key="index"
                        class="flex items-center relative">
                        
                        <h1 
                            ref="detailTexts"
                            class="text-5xl font-primary font-bold mb-4 transition-colors duration-300 ease-in-out cursor-pointer"
                            :style="{ color: activeIndex === index ? '#EEEEEE' : '#3A3C3E' }">
                            {{ item.text }}
                        </h1>
                    </div>
                </div>

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
    </div>
</template>

<script>
import { gsap } from 'gsap';

export default {
    data() {
        return {
            activeIndex: 0,
            lastScrollY: 0,
            detailItems: [  // change these to product details, or if one product then different images of the product
                {
                    text: 'DETAILS 1',
                    image: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c',
                    description: 'A vibrant and colorful salad bowl featuring fresh vegetables, grains, and a variety of textures. Perfect for a nutritious and satisfying meal that is as beautiful as it is delicious.'
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
            }
        }
    },
    mounted() {
        // Initialize all shapes
        this.initializeParallaxRect();
        this.initializeBlackRect();
        
        // Wait for DOM to be fully rendered AND images to load
        window.addEventListener('load', () => {
            this.calculateSectionPositions();
        });
        
        // Set up scroll event listener
        window.addEventListener('scroll', this.handleScroll);
        
        // Fallback in case images are already loaded
        if (document.readyState === 'complete') {
            this.calculateSectionPositions();
        }
        
        // Force initial scroll to top
        window.scrollTo(0, 0);
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.handleScroll);
        window.removeEventListener('load', this.calculateSectionPositions);
    },
    methods: {
        initializeParallaxRect() {
            if (this.$refs.parallaxRect) {
                // Set rectangle to be centered with initial size
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
        },
        
        initializeBlackRect() {
            if (this.$refs.blackRect) {
                // Position black rectangle to be under the image with part sticking out
                // The image is at left: 100px, top: 200px with width/height: 500px
                // So we position the black rectangle to be partially under it
                gsap.set(this.$refs.blackRect, {
                    width: this.blackRectInitialSize.width,
                    height: this.blackRectInitialSize.height,
                    left: '900px', // Position to overlap with image but stick out on right
                    top: '200px',  // Position to overlap with image but stick out on bottom
                    
                    zIndex: 5 // Ensure it's below the image (z-10) but above the yellow rectangle
                });
            }
        },
        
        calculateSectionPositions() {
            // Calculate the position of each section for scroll detection
            this.sectionPositions = [];
            
            this.detailItems.forEach((_, index) => {
                const sectionRef = this.$refs[`section${index}`];
                if (sectionRef && sectionRef[0]) {
                    const rect = sectionRef[0].getBoundingClientRect();
                    this.sectionPositions.push(rect.top + window.scrollY);
                }
            });
            
            // Initial check for active section
            this.handleScroll();
        },
        
        handleScroll() {
            const scrollY = window.scrollY;
            const windowHeight = window.innerHeight;
            const scrollMid = scrollY + (windowHeight / 2);
            
            // Determine which section is active based on scroll position
            let newActiveIndex = 0;
            for (let i = this.sectionPositions.length - 1; i >= 0; i--) {
                if (scrollMid >= this.sectionPositions[i]) {
                    newActiveIndex = i;
                    break;
                }
            }
            
            // Update active index if changed
            if (newActiveIndex !== this.activeIndex) {
                // Animate the text highlight transition
                this.animateTextHighlight(this.activeIndex, newActiveIndex);
                this.activeIndex = newActiveIndex;
                
                // Handle title visibility based on active index
                this.handleTitleVisibility(newActiveIndex);
            }
            
            // Parallax effect for rectangles
            this.updateParallaxRect(scrollY);
            this.updateBlackRect(scrollY);
            
            this.lastScrollY = scrollY;
        },
        
        handleTitleVisibility(activeIndex) {
            if (!this.$refs.titleContent || !this.$refs.scrollProgress) return;
            
            if (activeIndex === 0) {
                // Show title content with slide-in animation for DETAILS 1
                gsap.to(this.$refs.titleContent, {
                    x: 0,
                    opacity: 1,
                    duration: 0.7,
                    ease: "power2.out"
                });
                
                // Hide scroll progress
                gsap.to(this.$refs.scrollProgress, {
                    opacity: 0,
                    y: 20,
                    duration: 0.5,
                    ease: "power2.in"
                });
            } else {
                // Hide title content with slide-out animation for other DETAILS
                gsap.to(this.$refs.titleContent, {
                    x: -100,
                    opacity: 0,
                    duration: 0.7,
                    ease: "power2.in"
                });
                
                // Show scroll progress with animation
                gsap.to(this.$refs.scrollProgress, {
                    opacity: 1,
                    y: 0,
                    duration: 0.7,
                    delay: 0.2,
                    ease: "power2.out"
                });
            }
        },
        
        updateParallaxRect(scrollY) {
            if (!this.$refs.parallaxRect) return;
            
            // Calculate progress through the entire page (0 to 1)
            const docHeight = document.body.scrollHeight - window.innerHeight;
            const scrollProgress = scrollY / docHeight;
            
            // Create a simple expansion animation based on scroll progress
            const scaleAmount = 1 + (scrollProgress * 5); // Scale from 1x to 6x
            const opacityValue = 0.2 + (scrollProgress * 0.6); // Start low (0.2) and end high (0.8)
            
            gsap.to(this.$refs.parallaxRect, {
                scale: scaleAmount,
                opacity: opacityValue,
                duration: 0.5,
                ease: "power1.out"
            });
        },
        
        updateBlackRect(scrollY) {
            if (!this.$refs.blackRect) return;
            
            // Calculate progress through the entire page (0 to 1)
            const docHeight = document.body.scrollHeight - window.innerHeight;
            const scrollProgress = scrollY / docHeight;
            
            // Create a limited expansion animation based on scroll progress
            // Only scale up to 2x the original size
            const scaleAmount = 1 + Math.min(1, scrollProgress * 3); // Scale from 1x to max 2x
            
            gsap.to(this.$refs.blackRect, {
                scale: scaleAmount,
                duration: 0.5,
                ease: "power1.out"
            });
        },
        
        // Add a method to animate the text highlight transition
        animateTextHighlight(oldIndex, newIndex) {
            // Get the text elements
            const textElements = this.$refs.detailTexts;
            if (!textElements) return;
            
            // Animate the old text to dim
            if (textElements[oldIndex]) {
                gsap.to(textElements[oldIndex], {
                    color: '#3A3C3E',
                    duration: 0.3,
                    ease: 'power1.out'
                });
            }
            
            // Animate the new text to highlight
            if (textElements[newIndex]) {
                gsap.to(textElements[newIndex], {
                    color: '#EEEEEE',
                    duration: 0.3,
                    ease: 'power1.out'
                });
            }
        }
    }
}
</script>

<style scoped>
.detail-image {
    transition: opacity 0.5s ease-in-out;
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
</style>