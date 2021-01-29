<template>
    <div class="wrapper">
        <svg viewbox="0 0 800 800" >
            <defs>
                <clipPath style="transform: translateY(50px)" id="svgPath">
                    <!-- <path stroke="#000000" d="M45.479,17.482C33.7-10.258.22-3.785,0,28.411-0.123,46.094,15.942,52.7,26.638,59.773,37.01,66.628,44.392,76.007,45.549,80c0.99-3.912,9.212-13.555,18.813-20.418C74.86,52.077,91.123,45.9,91,28.219,90.779-4.057,56.716-9.151,45.479,17.482Z"/> -->
                    <path id="heart" d="M248.078,5.883c-36.691-14.739-77.771-0.839-98.517,31.125C128.817,5.044,87.735-8.856,51.043,5.883 C9.354,22.632-10.863,70.009,5.887,111.696c16.06,39.98,143.314,139.607,143.314,139.607l0.359,0.28l0.36-0.28 c0,0,127.251-99.627,143.314-139.607C309.986,70.009,289.768,22.632,248.078,5.883z"/>
                </clipPath>
            </defs>
        </svg>
        <svg class="lol" viewbox="0 0 800 800" height="300">
            <image class="svg-clipped" :xlink:href="url" height="300" ></image>
        </svg>
    </div>
</template>
<script>
export default {
    props: ['url']
}
</script>
<style lang="scss">
    .wrapper {
        position: absolute;
        // animation: pulse 3s ease infinite;
    }
    .lol {
        // opacity: .3;
        filter: sepia(1);
        opacity: .5;
    }
    .svg-clipped {
        -webkit-clip-path:url(#svgPath);
        clip-path:url(#svgPath);
        -moz-clip-path: url(#svgPath);
        -ms-clip-path: url(#svgPath);
        // opacity: .5;
        // transform: translateX(1px);
        // -webkit-transform: translate(1px);
        // -moz-transform: translate(1px);
        // -ms-transform: translate(1px);
        position: relative;
    }
    @keyframes pulse {
        0% { transform: rotate(-30deg) scale(1); }
        50% { transform: rotate(-30deg) scale(1.1); }
        100% { transform: rotate(-30deg) scale(1); }
    }
    
</style>