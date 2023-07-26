<script lang="ts">
  import {
    spring,
    timeline,
    type AnimationControls,
    type TimelineDefinition,
    type TimelineSegment,
  } from "motion";

  let animDir = "forward";
  let liked = false;
  let anim: AnimationControls;

  const f1: TimelineSegment = [
    ".heart-1",
    { scale: [1, 5] },
    { duration: 0.7 },
  ];
  const f2: TimelineDefinition = [
    [
      ".c1",
      {
        scale: [0, 4],
        backgroundColor: ["rgba(255,255,255,1)", "rgba(255,255,255,0)"],
      },
      { at: 0.0, duration: 2 },
    ],
    [".c2", { scale: [0, 10] }, { at: 0.0, duration: 1 }],
    [".heart-2", { scale: [0, 1] }, { at: 0.5, easing: spring() }],
  ];

  const b1: TimelineSegment = [
    ".heart-1",
    { scale: [5, 1] },
    { duration: 0.3 },
  ];
  const b2: TimelineDefinition = [
    [".c1", { scale: 0 }, { at: 0.0, duration: 0.1 }],
    [".heart-2", { scale: [1, 0] }, { duration: 0.2 }],
  ];

  function runAnim() {
    const sequence = [
      animDir === "forward" ? f1 : b1,
      ...(animDir === "forward" ? f2 : b2),
    ];

    anim = timeline(sequence);
    anim.finished.then(() => (anim = null!));
  }
</script>

<div class="hscreen flex-s-center">
  <button
    class="square-300px flex-s-center transition-450 b-(gray-2 4) md-square-500px
    br-100px hover-(shadow-2xl b-transparent) overflow-hidden group"
    disabled={!!anim}
    on:click={() => {
      liked = !liked;
      runAnim();
      animDir = animDir === "forward" ? "reverse" : "forward";
    }}
  >
    <div class="relative">
      <div
        class="heart-1 i-mdi-heart relative z-1 text-(100px gray) md-text-200px
        group-hover-text-red-5 [&.liked]-text-red-5"
        class:liked
      />

      <div class="abs top-0 left-0 z-2 square-full">
        <div class="c1 br-full square-98% bg-white flex-s-center scale-0">
          <div class="c2 br-full square-10% bg-red-5 flex-s-center" />
        </div>
      </div>

      <div class="abs z-2 square-full top-0 left-0">
        <div
          class="heart-2 i-mdi-heart scale-0 text-(100px white) md-text-200px"
        />
      </div>
    </div>
  </button>
</div>
