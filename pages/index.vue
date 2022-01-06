<template>
  <div>
    <Heading :updated-at="changedAt" :percent-rewritten="percentRewritten"/>
    <div class="stat-block">
      <LineCounts :java-stats="javaStats" :kotlin-stats="kotlinStats"/>
      <SizeGraph :percent="percentSizeChange"/>
    </div>
    <footer class="footer">
      Website and Project developed by <a href="https://jacobtread.com">Jacobtread</a>
    </footer>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export interface CodeStats {
  Name: string;
  CodeBytes: number;
  Lines: number;
  Code: number;
  Comment: number;
  Complexity: number;
  Count: number;
}

type StatsFile = CodeStats[];

interface Data {
  kotlinStats: CodeStats;
  javaStats: CodeStats;
  changedAt:  number;
  percentRewritten: number;
  percentSizeChange: number;
}

const emptyStats: CodeStats = {
  Name: "Empty",
  Code: 0,
  CodeBytes: 0,
  Lines: 0,
  Comment: 0,
  Complexity: 0,
  Count: 0
}

export default Vue.extend({
  name: 'IndexPage',
  data: (): Data => ({
    javaStats: emptyStats,
    kotlinStats: emptyStats,
    changedAt: 0,
    percentRewritten: 0,
    percentSizeChange: 0
  }),
  fetchOnServer: true,
  async fetch(): Promise<void> {
    if (process.server) {
      const fs = require('fs');

      const rawData = fs.readFileSync('stats/scc.json', 'utf-8')
      const data: StatsFile = JSON.parse(rawData)

      const getOrDefault = (name: string): CodeStats => {
        const value = data.find((stats: CodeStats) => stats.Name == name)
        if (!value) return emptyStats
        else return value
      }

      let javaStats: CodeStats = getOrDefault("Java")
      let kotlinStats: CodeStats = getOrDefault("Kotlin")

      this.javaStats = javaStats
      this.kotlinStats = kotlinStats

      const originalLines = 272036;
      const removedLines = originalLines - this.javaStats.Code
      const kotlinSizeChange = 1 - (this.kotlinStats.Code / removedLines)

      this.percentRewritten = (removedLines / originalLines) * 100;
      this.percentSizeChange = (kotlinSizeChange * 100);
      this.changedAt = new Date().getTime()
    }
  }
})
</script>

<style lang="scss" scoped>
.stat-block {
  display: flex;
  justify-content: space-between;
  max-width: 900px;
  padding: 2rem;
  gap: 1rem;
  margin: 5rem auto;
  flex-wrap: wrap-reverse;
  align-items: center;
}

.footer {
  display: block;
  width: 100%;
  padding: 0.5rem;
  color: #666666;

  a {
    color: #7472E2;
  }
}
</style>
