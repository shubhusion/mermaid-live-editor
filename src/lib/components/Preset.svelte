<script lang="ts">
  import { updateCode } from '$lib/util/state';
  import Card from '$lib/components/Card/Card.svelte';
  import { logEvent } from '$lib/util/stats';

  const samples = {
    Flow: `flowchart TD
    A[Christmas] -->|Get money| B(Go shopping)
    B --> C{Let me think}
    C -->|One| D[Laptop]
    C -->|Two| E[iPhone]
    C -->|Three| F[fa:fa-car Car]`,
    Sequence: `sequenceDiagram
    Alice->>+John: Hello John, how are you?
    Alice->>+John: John, can you hear me?
    John-->>-Alice: Hi Alice, I can hear you!
    John-->>-Alice: I feel great!`,
    Class: `classDiagram
    Animal <|-- Duck
    Animal <|-- Fish
    Animal <|-- Zebra
    Animal : +int age
    Animal : +String gender
    Animal: +isMammal()
    Animal: +mate()
    class Duck{
      +String beakColor
      +swim()
      +quack()
    }
    class Fish{
      -int sizeInFeet
      -canEat()
    }
    class Zebra{
      +bool is_wild
      +run()
    }`,
    State: `stateDiagram-v2
    [*] --> Still
    Still --> [*]
    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]`,
    Gantt: `gantt
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    A task           :a1, 2014-01-01, 30d
    Another task     :after a1  , 20d
    section Another
    Task in sec      :2014-01-12  , 12d
    another task      : 24d`,
    Pie: `pie title Pets adopted by volunteers
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15`,
    ER: `erDiagram
    CUSTOMER }|..|{ DELIVERY-ADDRESS : has
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER ||--o{ INVOICE : "liable for"
    DELIVERY-ADDRESS ||--o{ ORDER : receives
    INVOICE ||--|{ ORDER : covers
    ORDER ||--|{ ORDER-ITEM : includes
    PRODUCT-CATEGORY ||--|{ PRODUCT : contains
    PRODUCT ||--o{ ORDER-ITEM : "ordered in"`,
    'User Journey': `journey
    title My working day
    section Go to work
      Make tea: 5: Me
      Go upstairs: 3: Me
      Do work: 1: Me, Cat
    section Go home
      Go downstairs: 5: Me
      Sit down: 3: Me`,
    Git: `gitGraph
    commit
    commit
    branch develop
    checkout develop
    commit
    commit
    checkout main
    merge develop
    commit
    commit`,
    Mindmap: `mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectivness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid`,
    QuadrantChart: `quadrantChart
    title Reach and engagement of campaigns
    x-axis Low Reach --> High Reach
    y-axis Low Engagement --> High Engagement
    quadrant-1 We should expand
    quadrant-2 Need to promote
    quadrant-3 Re-evaluate
    quadrant-4 May be improved
    Campaign A: [0.3, 0.6]
    Campaign B: [0.45, 0.23]
    Campaign C: [0.57, 0.69]
    Campaign D: [0.78, 0.34]
    Campaign E: [0.40, 0.34]
    Campaign F: [0.35, 0.78]`
  };

  type SampleTypes = keyof typeof samples;
  const loadSampleDiagram = (diagramType: SampleTypes): void => {
    updateCode(samples[diagramType], {
      updateDiagram: true,
      resetPanZoom: true
    });
    logEvent('loadSampleDiagram', { diagramType });
  };

  // Adding in this array will add an icon to the preset menu
  const newDiagrams: SampleTypes[] = ['Mindmap', 'QuadrantChart'];
  const diagramOrder: SampleTypes[] = [
    'Sequence',
    'Flow',
    'Class',
    'State',
    'ER',
    'Gantt',
    'User Journey',
    'Git',
    'Pie',
    'Mindmap',
    'QuadrantChart'
  ];
</script>

<Card title="Sample Diagrams" isOpen={false}>
  <div class="flex flex-wrap p-2 gap-2">
    {#each diagramOrder as sample}
      <button
        class="btn btn-sm btn-primary w-28 normal-case flex-grow"
        on:click={() => loadSampleDiagram(sample)}>
        {sample}
        {#if newDiagrams.includes(sample)}
          <span class="ml-2 fa fa-heart" />
        {/if}
      </button>
    {/each}
  </div>
</Card>
