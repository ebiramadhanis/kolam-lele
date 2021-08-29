<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6" class="d-flex justify-center align-center">
      <v-row>
        <v-col cols="6" sm="6" offset-sm="3">
          <v-card height="200px">
            <v-card-title class="blue white--text">
              <span class="text-h5">Ph Air</span>
            </v-card-title>

            <v-card-text>
              <div class="d-flex justify-center align-center text-h4">
                Ph Air
              </div>
              <div class="d-flex justify-center align-center text-h4">
                {{ db.ph }}
              </div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="6" sm="6" offset-sm="3">
          <v-card height="200px">
            <v-card-title class="blue white--text">
              <span class="text-h5">Servo</span>
            </v-card-title>

            <v-card-text>
              <div
                class="d-flex flex-column justify-center align-center"
                height="100%"
              >
                <div class="text-h4" v-if="servo">ON</div>
                <div class="text-h4" v-else>OFF</div>
                <v-switch inset v-model="servo" hide-details></v-switch></div
            ></v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>


<script>
export default {
  data() {
    return {
      ph: 0,
      servo: false,
      db: {
        ph: 0,
        servo: false,
      },
    };
  },
  mounted() {
    this.fetchDb();
  },
  watch: {
    ph(val) {
      this.refPh.set(val);
    },
    servo(val) {
      this.refServo.set(val);
    },
  },
  computed: {
    refPh() {
      return this.$fire.database.ref().ref.child("ph");
    },
    refServo() {
      return this.$fire.database.ref().ref.child("servo");
    },
  },
  methods: {
    fetchDb() {
      this.refPh.on("value", (dataSnapshot) => {
        this.db.ph = dataSnapshot.val();
      }),
        this.refServo.on("value", (dataSnapshot) => {
          this.db.servo = dataSnapshot.val();
        });
    },
  },
};
</script>