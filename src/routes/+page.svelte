<script lang='ts'>
    import { LondonData } from "./population_london";
    
    const data = LondonData.filter(_=>_.Year == 2023)
    const density = data.map(_=>_.Population_per_square_kilometre)

    // first, we set the height and width of the svg graph
    // scaleFactor adjusts the height of the bar
    let scaleFactor = 200
    $: scaling = Math.max(...density) / scaleFactor
    $: heightArray = data.map(_=>_.Population_per_square_kilometre / scaling)
    $: maxHeight = Math.max(...heightArray)*1.1   
    // then we adjust the bar width. the graph width depends on this
    let barwidth = 1500
    $: width = data.length * barwidth / 100
    
    // then establish a borough of interest. let's set it on default on index 8.
    let selectionIndex = 8
    $: selectedDensity = density[selectionIndex]

    // finally, if user clicks on the bar, activate the bar.
    function activateSelection(num: number){
        selectionIndex = num
    }
</script>

<h1>London Population Data 2023</h1>
<div id='flexbox'>
    <div style='text-align: center;'>
        <div>
            density of
            <select bind:value={selectionIndex}>
                {#each data as d,i}
                    <option value={i}>{d.Name}</option>
                {/each}
            </select>
            is {selectedDensity} per square km
        </div>
        <div>
            scale the height:
            <br>
            <input type="range" min=10 max=600 bind:value={scaleFactor} id="heightslider">
            <br>
            scale the width:
            <br>
            <input type="range" min=500 max=3000 bind:value={barwidth} id="widthslider" >
        </div>
    </div>
    <div>
        <svg width={data.length * barwidth / 100} height={maxHeight} >
            <g >
                {#each data as d,i}
                <rect  height={d.Population_per_square_kilometre / scaling} y={maxHeight - d.Population_per_square_kilometre / scaling} width={barwidth/100*0.8}  x={(i * barwidth / 100 * 1)} fill={(i == selectionIndex)? "black": "grey"} on:click={()=>{return activateSelection(i)}} on:keydown={()=>{}}></rect>
                {/each}
            </g>
        </svg>
    </div>
    <div style='margin: 10px'>
        <a href="https://data.london.gov.uk/dataset/land-area-and-population-density-ward-and-borough">data source</a>
    </div>
    <div style='margin: 10px'>
        created by <a href="https://sutan.co.uk">Sutan.co.uk</a>
    </div>
</div>


<style>
h1 {
    text-align: center;
}
div {
    margin: 0 15px 0 0;
}

rect {
    cursor: pointer;
}

#flexbox {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
</style>

