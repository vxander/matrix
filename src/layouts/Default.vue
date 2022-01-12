<template>
  <v-app>
    <v-app-bar app> </v-app-bar>
    <v-main>
      <v-container fluid>
        <v-row>
          <v-col sm="4" no-gutters>
            <v-row no-gutters>
              <v-card color="#385F73">
                <v-date-picker
                  @click:date="compute"
                  full-width
                  header-color="#3399ff"
                  color="#3399ff"
                  v-model="picker"
                  locale="ru-ru"
                >
                </v-date-picker>
              </v-card>
            </v-row>
            <v-row no-gutters>
              <v-card color="#385F73" width="100%" class="mt-5" >
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th>Линия Земли</th>
                        <th class="text-left">Линия Неба</th>
                        <th class="text-left">Энергия</th>
                        <th class="text-left">Чакры</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="item in table"
                        :key="item.id"
                        :bgcolor="item.color"
                      >
                        <td>{{ item.line1 }}</td>
                        <td>{{ item.line2 }}</td>
                        <td>{{ item.line3 }}</td>
                        <td>{{ item.name }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-card>
            </v-row>
          </v-col>
          <v-col sm="7"
            no-gutters
            align-self="center"
          >
            <v-card
              min-width="300"

            >
              <svg
                :viewBox="
                  [
                    -diag - radius * 2,
                    -diag - radius * 2,
                    2 * diag + radius * 4,
                    2 * diag + radius * 4,
                  ].join(' ')
                "
              >
                <line
                  v-for="line in lines"
                  :key="line.id"
                  :x1="line.x1"
                  :y1="-line.y1"
                  :x2="line.x2"
                  :y2="-line.y2"
                  stroke="#3399ff"
                  stroke-width="1"
                />
                <circle
                  v-for="node in nodes"
                  :key="node.id"
                  :cx="node.x"
                  :cy="-node.y"
                  :r="node.r"
                  stroke="black"
                  stroke-width="1"
                  :fill="node.color"
                />
                <text
                  v-for="node in nodes"
                  :x="node.x"
                  :key="'c' + node.id"
                  :y="-node.y"
                  :id="node.id"
                  text-anchor="middle"
                  dominant-baseline="middle"
                  font-size="smaller"
                  fill="black"
                >
                  {{ node.value }}
                </text>
              </svg>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer app> </v-footer>
  </v-app>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<style>
</style>
<script>
const side = 300;
const center = 0;
const radius = side / 30;
const half = side / 2;
const diag = Math.sqrt(half * half + half * half);
const diag2 = half + (diag - half) / 2;

const lines_raw = [
  {
    id: "l1",
    start: "n1",
    end: "n2",
    color: "",
  },
  {
    id: "l2",
    start: "n2",
    end: "n3",
    color: "",
  },
  {
    id: "l3",
    start: "n3",
    end: "n4",
    color: "",
  },
  {
    id: "l4",
    start: "n4",
    end: "n1",
    color: "",
  },
  {
    id: "l5",
    start: "n1",
    end: "n3",
    color: "",
  },
  {
    id: "l6",
    start: "n2",
    end: "n4",
    color: "",
  },
  {
    id: "l7",
    start: "n6",
    end: "n7",
    color: "",
  },
  {
    id: "l8",
    start: "n8",
    end: "n5",
    color: "",
  },
  {
    id: "l9",
    start: "n9",
    end: "n10",
    color: "",
  },
  {
    id: "l10",
    start: "n10",
    end: "n11",
    color: "",
  },
  {
    id: "l11",
    start: "n11",
    end: "n12",
    color: "",
  },
  {
    id: "l12",
    start: "n12",
    end: "n9",
    color: "",
  },
  {
    id: "l13",
    start: "n9",
    end: "n11",
    color: "",
  },
  {
    id: "l14",
    start: "n10",
    end: "n12",
    color: "",
  },
  {
    id: "l15",
    start: "n13",
    end: "n14",
    color: "",
  },
];

const nodes_raw = [
  {
    id: "n9",
    order: 1,
    x: center,
    y: center + diag,
    color: "#cb65ff",
    r: radius * 1.5,
    rule: [],
    value: "",
  },
  {
    id: "n12",
    order: 2,
    x: center - diag,
    y: center,
    color: "#cb65ff",
    r: radius * 1.5,
    rule: [],
    value: "",
  },
  {
    id: "n10",
    order: 3,
    x: center + diag,
    y: center,
    color: "#ff6565",
    r: radius * 1.5,
    rule: [],
    value: "",
  },
  {
    id: "n11",
    order: 4,
    x: center,
    y: center - diag,
    color: "#ff6565",
    r: radius * 1.5,
    rule: [1, 2, 3],
    value: "",
  },
  {
    id: "n1",
    order: 5,
    x: center - half,
    y: center + half,
    color: "#eaeaeb",
    r: radius,
    rule: [1, 2],
    value: "",
  },
  {
    id: "n2",
    order: 6,
    x: center + half,
    y: center + half,
    color: "#eaeaeb",
    r: radius,
    rule: [1, 3],
    value: "",
  },
  {
    id: "n3",
    order: 7,
    x: center + half,
    y: center - half,
    color: "#eaeaeb",
    r: radius,
    rule: [3, 4],
    value: "",
  },
  {
    id: "n4",
    order: 8,
    x: center - half,
    y: center - half,
    color: "#eaeaeb",
    r: radius,
    rule: [2, 4],
    value: "",
  },
  {
    id: "n28_1",
    order: 9,
    x: center,
    y: center + radius,
    color: "#65ff65",
    r: 0,
    rule: [2, 1, 3, 4],
    value: "",
  },
  {
    id: "n28_2",
    order: 10,
    x: center,
    y: center - radius,
    color: "#65ff65",
    r: 0,
    rule: [5, 6, 7, 8],
    value: "",
  },

  {
    id: "n21",
    order: 11,
    x: center - (diag - half),
    y: center + half,
    color: "#eaeaeb",
    r: radius,
    rule: [5, 1],
    value: "",
  },
  {
    id: "n20",
    order: 12,
    x: center - half,
    y: center + diag - half,
    color: "#eaeaeb",
    r: radius,
    rule: [5, 2],
    value: "",
  },
  {
    id: "n22",
    order: 13,
    x: center + diag - half,
    y: center + half,
    color: "#eaeaeb",
    r: radius,
    rule: [1, 6],
    value: "",
  },
  {
    id: "n23",
    order: 14,
    x: center + half,
    y: center + diag - half,
    color: "#eaeaeb",
    r: radius,
    rule: [3, 6],
    value: "",
  },
  {
    id: "n24",
    order: 15,
    x: center + half,
    y: center - (diag - half),
    color: "#eaeaeb",
    r: radius,
    rule: [3, 7],
    value: "",
  },
  {
    id: "n25",
    order: 16,
    x: center + diag - half,
    y: center - half,
    color: "#eaeaeb",
    r: radius,
    rule: [4, 7],
    value: "",
  },
  {
    id: "n26",
    order: 17,
    x: center - (diag - half),
    y: center - half,
    color: "#eaeaeb",
    r: radius,
    rule: [4, 8],
    value: "",
  },
  {
    id: "n27",
    order: 18,
    x: center - half,
    y: center - (diag - half),
    color: "#eaeaeb",
    r: radius,
    rule: [2, 8],
    value: "",
  },
  {
    id: "n5",
    order: 19,
    x: center,
    y: center + half,
    color: "#99cbff",
    r: radius * 1.1,
    rule: [1, 9],
    value: "",
  },
  {
    id: "n6",
    order: 20,
    x: center + half,
    y: center,
    color: "#fefdae",
    r: radius,
    rule: [3, 9],
    value: "",
  },
  {
    id: "n7",
    order: 21,
    x: center,
    y: center - half,
    color: "#fefdae",
    r: radius,
    rule: [4, 9],
    value: "",
  },
  {
    id: "n8",
    order: 22,
    x: center - half,
    y: center,
    color: "#99cbff",
    r: radius * 1.1,
    rule: [2, 9],
    value: "",
  },
  {
    id: "n17",
    order: 23,
    x: center - half / 2,
    y: center + half / 2,
    color: "#99cbff",
    r: radius * 1.1,
    rule: [19, 22],
    value: "",
  },
  {
    id: "n18",
    order: 24,
    x: center + half / 2,
    y: center - half / 2,
    color: "#fefdae",
    r: radius,
    rule: [20, 21],
    value: "",
  },
  {
    id: "n16",
    order: 25,
    x: center,
    y: center + diag2,
    color: "#3399ff",
    r: radius * 1.3,
    rule: [1, 19],
    value: "",
  },
  {
    id: "n13",
    order: 26,
    x: center + diag2,
    y: center,
    color: "#ffcb99",
    r: radius * 1.3,
    rule: [3, 20],
    value: "",
  },
  {
    id: "n14",
    order: 27,
    x: center,
    y: center - diag2,
    color: "#ffcb99",
    r: radius * 1.3,
    rule: [4, 21],
    value: "",
  },
  {
    id: "n15",
    order: 28,
    x: center - diag2,
    y: center,
    color: "#3399ff",
    r: radius * 1.3,
    rule: [2, 22],
    value: "",
  },
  {
    id: "n19",
    order: 29,
    x: center + diag2 / 2,
    y: center - diag2 / 2,
    color: "#ffcb99",
    r: radius,
    rule: [26, 27],
    value: "",
  },
  {
    id: "n28_3",
    order: 30,
    x: center,
    y: center,
    color: "#65ff65",
    r: radius * 2,
    rule: [],
    value: "",
  },
];

const table_raw = [
  {
    id: 1,
    name: "Сахасрара",
    line1: "",
    line2: "",
    line3: "",
    color: "#cb65ff",
    rule:['n12','n9'],
  },
  {
    id: 2,
    name: "Аджна",
    line1: "",
    line2: "",
    line3: "",
    color: "#3399ff",
    rule:['n15','n16'],
  },
  {
    id: 3,
    name: "Вишудха",
    line1: "",
    line2: "",
    line3: "",
    color: "#99cbff",
    rule:['n8','n5'],
  },
  {
    id: 4,
    name: "Анахата",
    line1: "",
    line2: "",
    line3: "",
    color: "#65ff65",
    rule:['n28_1','n28_1'],
  },
  {
    id: 5,
    name: "Манипура",
    line1: "",
    line2: "",
    line3: "",
    color: "#fefdae",
    rule:['n6','n7'],
    
  },
  {
    id: 6,
    name: "Свадхистана",
    line1: "",
    line2: "",
    line3: "",
    color: "#ffcb99",
    rule:['n13','n14'],
  },
  {
    id: 7,
    name: "Муладхара",
    line1: "",
    line2: "",
    line3: "",
    color: "#ff6565",
    rule:['n10','n11'],
  },
];

function sun(n) {
  if (typeof n === "string") {
    n = parseInt(n);
  }
  while (n > 22) {
    n = n
      .toString()
      .split("")
      .map(Number)
      .reduce((a, b) => a + b, 0);
  }
  return n;
}

export default {
  methods: {
    compute() {
      const date = this.picker.split("-");
      const year = date[0];
      const month = date[1];
      const day = date[2];

      this.nodes[0].value = sun(month);
      this.nodes[1].value = sun(day);
      this.nodes[2].value = sun(year);

      for (let i = 3; i < this.nodes.length - 1; i++) {
        this.nodes[i].value = sun(
          this.nodes[i].rule
            .map((x) => this.nodes[x - 1].value)
            .reduce((a, b) => a + b, 0)
        );
      }


      for (let i = 0; i < this.table.length; i++)  {

        const a_id = this.table[i].rule[0];
        const b_id = this.table[i].rule[1];
        const node_a = this.nodes.find((x) => x.id === a_id);
        const node_b = this.nodes.find((x) => x.id === b_id);
        this.table[i].line1 = node_a.value;
        this.table[i].line2 = node_b.value;
        this.table[i].line3 = sun(node_a.value + node_b.value);
      }
      
    },
  },
  data() {
    return {
      picker: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
      diag: diag,
      radius: radius,
      lines: lines_raw.map((line_raw) => {
        const node_start_id = nodes_raw.find((x) => x.id === line_raw.start);
        const node_end_id = nodes_raw.find((x) => x.id === line_raw.end);
        const line = {
          id: line_raw.id,
          x1: node_start_id.x,
          y1: node_start_id.y,
          x2: node_end_id.x,
          y2: node_end_id.y,
          color: line_raw.color,
        };
        return line;
      }),
      nodes: nodes_raw,

      table: table_raw,
    };
  },
};
</script>