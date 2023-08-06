<template>
  <!-- <h2>Hey there dude {{ note }}</h2> -->
  <div class="display_panel" id="display_panel">
     <md-content class="md-elevation-10">
       <canvas id='display'></canvas>
      </md-content>
    </div>
</template>

<script>
import { Note } from "tonal";
import Vex from 'vexflow';

const VF = Vex.Flow
export default {
  mounted() {
    // Create a Canvas renderer and attach it to the DIV element named "display".
    var div = document.getElementById("display")
    var renderer = new VF.Renderer(div, VF.Renderer.Backends.CANVAS);
    // Size our canvas:
    renderer.resize(500, 500);
    // And get a drawing context:
    var context = renderer.getContext();
    // Create a stave at position 10, 40 of width 400 on the canvas.
    var stave = new VF.Stave(10, 40, 400);
    // Add a clef and time signature.
    stave.addClef("treble").addTimeSignature("4/4");
    // Connect it to the rendering context and draw!
    stave.setContext(context).draw();
    var notes = [
    // A quarter-note C.
    new VF.StaveNote({clef: "treble", keys: ["c/4"], duration: "q" }),
    // A quarter-note D.
    new VF.StaveNote({clef: "treble", keys: ["d/4"], duration: "q" }),
    // A quarter-note rest. Note that the key (b/4) specifies the vertical position of the rest.
    new VF.StaveNote({clef: "treble", keys: ["b/4"], duration: "qr" }),
    // A C-Major chord.
    new VF.StaveNote({clef: "treble", keys: ["c/4", "e/4", "g/4"], duration: "q" })
];
    // Create a voice in 4/4 and add above notes
    var voice = new VF.Voice({num_beats: 4,  beat_value: 4});
    voice.addTickables(notes);
    // Format and justify the notes to 400 pixels.
    new VF.Formatter().joinVoices([voice]).format([voice], 400);
      // Render voice
      voice.draw(context, stave);
  },
  data(){
    return {
      ctx: null,
      name: 'Display',
      note: Note.freq("a4"),
    }
  }
}
</script>

<style lang="scss" scoped>
  .md-content {
    background:#d9dad5;
    margin-left: 175px;
    margin-right: 175px;
    padding-left: 100px;
    margin-top: 100px;
    display: flex;
    justify-content: left;
}
</style>