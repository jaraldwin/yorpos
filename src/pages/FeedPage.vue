<template>
  <div>
    <div align="right" class="q-ma-lg">
      <q-btn
        align="right"
        style="background-color: #d8e6e9 !important"
        icon="add"
        label="Add Product"
        @click="addModal()"
        flat
        dense
      >
      </q-btn>
    </div>
    <div>
      <!-- <q-file v-model="fileme" label="Standard" />
      <q-btn
        color="accent"
        label="Upload"
        class="q-pa-lg"
        @click="uplaod()"
        flat
        dense
      /> -->

      <q-dialog v-model="openedAddModal">
        <q-card class="q-ma-md" width="500px">
          <q-card-section>
            <q-input
              label="Name"
              lazy-rules
              :rules="[(val) => !!val || 'Please enter  Name']"
              v-model="name"
            >
            </q-input>

            <q-input v-model="specifications" label="Specifications"> </q-input>
            <q-input v-model.number="price" label="Price"> </q-input>
            <q-select v-model="category" :options="optionsCategory" label="Category" />

            <q-separator />
            <br />
            <q-file
              filled
              bottom-slots
              v-model="fileme"
              label="Product Image"
              counter
            >
              <template v-slot:prepend>
                <q-icon name="cloud_upload" @click.stop.prevent />
              </template>
              <template v-slot:append>
                <q-icon
                  name="close"
                  @click.stop.prevent="fileme = null"
                  class="cursor-pointer"
                />
              </template>

              <template v-slot:hint> Product Pic </template>
            </q-file>
            <br />
          </q-card-section>
          <q-card-actions align="right">
            <q-btn
              color="accent"
              label="ADD Product"
              class="q-pa-lg"
              @click="Add()"
              flat
              dense
            />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
    <!-- now -->
    <div class="row">
      <div class="col-9 row" style="align-items: flex-start">
        <q-tabs
          class="q-ma-sm col-11 row"
          align="justify"
          :breakpoint="0"
          v-model="tab"
        ><q-tab value="all" name="all" icon="list" label="all"  class="hover-tab"/>

          <q-tab value="starter" name="starter" icon="lunch_dining" label="Starter"  class="hover-tab"/>
          <q-tab value="drinks" name="drinks" icon="sports_bar" label="Drinks" class="hover-tab" />
          <q-tab value="promos" name="promos" icon="fastfood" label="Promos" class="hover-tab" />
        </q-tabs>
        <q-card
          style="background-color: #d8e6e9 !important"
          @mouseover="targetEl = true"
          v-for="item in datassss"
          :key="item.id"
          class="q-ma-md"
        >
          <q-img
            width="300px"
            @click="addOrder(item)"
            height="250px"
            :src="item.image"
          >
            <q-tooltip
              class="q-pl-lg q-pr-lg"
              v-if="targetEl"
              :target="targetEl"
              anchor="center middle"
              self="center middle"
              style="background-color: #d89f65"
              ><span class="text-weight-regular text-h5 text-black"
                >ADD</span
              ></q-tooltip
            >
          </q-img>
          <q-card-section>
            <div
              align="center"
              class="text-h6 text-uppercase"
              style="
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
                max-width: 250px;
              "
            >
              {{ item.name }}
              <span class="text-h6 text-uppercase">&#8369;</span>
              {{ item.price }}
            </div>
            <div>
              <span class="text-uppercase">Category:</span>
              <q-badge rounded color="amber-10" class="q-ml-lg">{{
                item.category
              }}</q-badge>
            </div>
          </q-card-section>
          <q-card-section align="right">
            <q-btn
              color="secondary"
              class="q-mr-md"
              icon="edit"
              @click="editData(item)"
            />
            <q-btn
              color="negative"
              icon="close"
              hidden
              @click="openedpassData(item)"
            />
          </q-card-section>
        </q-card>
      </div>
      <!-- NEXT -->
      <div class="col row">
        <div>
          <q-card
            flat
            bordered
            class="my-card"
            :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'"
          >
            <!-- nasa -->
            <q-card-section>
              <div>
                <div>
                  <div class="text-h6">
                    ORDER#
                    <q-badge
                      color="blue"
                      align="right"
                      v-if="orders.length !== 0"
                    >
                      {{ currentYear }}{{ uuid.slice(0, 5) }}
                    </q-badge>
                  </div>
                  <q-separator inset />
                  <div
                    style="text-align: center"
                    v-if="orders.length === 0"
                    class="text-subtitle2 q-ma-sm"
                  >
                    <img
                      class=""
                      align="center"
                      v-if="$q.platform.is.desktop"
                      alt="Quasar logo"
                      width="150"
                      height="150"
                      src="~assets/noitem.png"
                    />
                  </div>
                  <div v-else>
                    <div
                      v-for="order in orders"
                      :key="order.id"
                      class="text-subtitle2 q-ma-md q-pa-sm"
                    >
                      <q-card
                        bordered
                        class="my-card"
                        style="background-color: #eaf0f0 !important"
                      >
                        <q-item>
                          <q-item-section avatar>
                            <img
                              class="border-radius-inherit"
                              :src="order.image"
                              alt="order iamge"
                              width="70"
                              height="70"
                            />
                          </q-item-section>
                          <q-item-section>
                            <q-item-label> {{ order.name }} </q-item-label>
                            <q-item-label class="text-h5">
                              {{ order.price }}
                            </q-item-label>
                          </q-item-section>
                          <q-item-section>
                            <q-item-label> QUANTITY </q-item-label>
                            <q-label class="text-h5" align="center"
                              >{{ order.order_quantity }}
                            </q-label>
                          </q-item-section>
                          <q-item-section>
                            <q-label class="text-h5">
                              <q-btn
                                flat
                                dense
                                icon="add"
                                @click="increaseQuantity(order)"
                              ></q-btn>
                              <q-btn
                                flat
                                dense
                                icon="remove"
                                @click="decreaseQuantity(order)"
                              ></q-btn>
                            </q-label>
                          </q-item-section>
                        </q-item>
                      </q-card>
                    </div>
                  </div>
                </div>
              </div>
            </q-card-section>
            <q-separator inset />

            <q-card class="my-card">
              <q-item>
                <q-card-section>
                  <q-item-section>
                    <q-item-label class="text-h5">
                      TOTAL AMOUNT:
                    </q-item-label>
                  </q-item-section>
                </q-card-section>
                <q-card-section>
                  <q-item-section>
                    <q-item-label class="text-h5">
                      {{ totalAmount }}
                    </q-item-label>
                  </q-item-section>
                </q-card-section>
              </q-item>

              <q-separator />

              <q-card-actions>
                <q-btn
                  @click="cancelOrder"
                  color="deep-orange"
                  glossy
                  label="Cancel Order"
                />
                <q-btn @click="placeOrder" color="brown-5" label="Send Order" />
              </q-card-actions>
            </q-card>
          </q-card>
        </div>
      </div>
      <q-dialog persistent v-model="editModal" width="700px">
        <q-card style="width: 100%; max-width: 800px; padding: 20px">
          <div class="text-h6">Edit Product</div>
          <q-card-section>
            <div class="text-subtitle2">
              <q-input v-model="name" label="Item name" />
              <q-input v-model="specifications" label="Specifications" />
              <q-input v-model="price" label="Price" />
              <q-file
                filled
                bottom-slots
                v-model="fileme"
                label="Product Image"
                counter
              >
                <template v-slot:prepend>
                  <q-icon name="cloud_upload" @click.stop.prevent />
                </template>
                <template v-slot:append>
                  <q-icon
                    name="close"
                    @click.stop.prevent="fileme = null"
                    class="cursor-pointer"
                  />
                </template>
              </q-file>
              <div v-if="fileme">
                <img
                  style="max-width: 200px; max-height: 200px"
                  :src="fileme"
                  alt="productImage"
                />
              </div>
            </div>
          </q-card-section>

          <q-card-actions>
            <q-btn
              label="Cancel"
              type="reset"
              v-close-popup
              class="col"
              unelevated
              @click="cancelEdit"
            />
            <q-btn
              unelevated
              @click="saveEdit"
              type="submit"
              label="Save"
              class="col"
              color="primary"
            />
          </q-card-actions>
        </q-card>
      </q-dialog>

      <!-- delete item -->
      <q-dialog v-model="opened">
        <q-card class="my-card">
          <q-card-section>
            <div class="text-subtitle2">
              Are your sure you want do delete this item {{ docName }}?
            </div>
          </q-card-section>

          <q-card-actions>
            <q-btn flat v-close-popup="opened">Cancel</q-btn>
            <q-btn flat @click="confirmed">Confirm</q-btn>
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
  </div>
</template>

<script setup>
import { nanoid } from "nanoid";
import {
  onMounted,
  computed,
  ref,
  watch,
  getCurrentInstance,
  onBeforeMount,
  onServerPrefetch,
} from "vue";
import { useRouter } from "vue-router";
import { useQuasar, QSpinnerGears } from 'quasar'
import { initializeApp, getApp } from "firebase/app";
import {
  getFirestore,
  collection,
  addDoc,
  onSnapshot,
  query,
  where,
  doc,
  deleteDoc,
  updateDoc,
} from "firebase/firestore";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries
import {
  ref as storageRef,
  getDownloadURL as stogegetDownloadURL,
} from "@firebase/storage";
console.log("NANOID", nanoid());
import { getAuth, onAuthStateChanged, signOut } from "@firebase/auth";
const app = getCurrentInstance().appContext.config.globalProperties;

// Your web app's Firebase configuration
const datassss = ref([]);
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const description = ref("");
const name = ref("");
const specifications = ref("");
const price = ref(0);
const pricebefore = ref(0);
const ratings = ref(2.3);
const category = ref("");
const tab = ref("all")
const q = ref(null); // Placeholder for the Firestore query
    const unsubscribe = ref(null); // Placeholder for the unsubscribe function
const optionsCategory = ref( [
        'starter', 'drinks', 'promos',
      ])
const order_quatity = 0;
const images = "";
const opened = ref(false);
const docId = ref("");
const docName = ref("");
const docDesc = ref("");
const fileme = ref([]);
const image = ref("");
const editModal = ref(false);
const openedAddModal = ref(false);
const uuid = ref("");
const firstFiveCharacters = uuid.value.slice(0, 5);
const orders = ref([]);
const grandTot = ref(0);
const currentYear = new Date().getFullYear();

const firebaseConfig = {
  apiKey: "AIzaSyDbjhOcP2TgjTn1Me9NxaGLJYjF8i8ktZE",
  authDomain: "fire-tut-981d2.firebaseapp.com",
  databaseURL:
    "https://fire-tut-981d2-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "fire-tut-981d2",
  storageBucket: "fire-tut-981d2.appspot.com",
  messagingSenderId: "935943220581",
  appId: "1:935943220581:web:e551613f36c35b1c41058c",
  measurementId: "G-TH1TFB65DG",
};

// Initialize Firebase
const app1 = initializeApp(firebaseConfig);
const db = getFirestore(app1);
const storage = app.$getStorage();

const editedItem = ref(null);
const targetEl = ref(false);

async function addModal() {
  openedAddModal.value = true;
}
const router = useRouter();
const isLoggedIn = ref(true);
const $q = useQuasar()
let auth;

async function Add() {
  const forestRef = ref(storage, fileme.value.name.toString());
  console.log("forestRef", forestRef);
  const mountainsRef = storageRef(storage, fileme.value.name.toString());
  await app.$uploadBytes(mountainsRef, fileme.value).then((snapshot) => {
    console.log("storage", snapshot);
  });
  try {
    await stogegetDownloadURL(
      storageRef(storage, fileme.value.name.toString())
    ).then((url) => {
      console.log(url);
      image.value = url;
    });
    const docRef = await addDoc(collection(db, "iam"), {
      name: name.value,
      description: description.value,
      specifications: specifications.value,
      price: price.value,
      pricebefore: pricebefore.value,
      ratings: ratings.value,
      category: category.value,
      image: image.value,
      ratings: ratings.value,
      percent: (price.value / pricebefore.value) * 100,
      userid: uuid.value,
    });
    openedAddModal.value = false;
    (name.value = ""),
      (description.value = ""),
      console.log("Document written with ID: ", docRef.id);
  } catch (e) {
    console.error("Error adding document: ", e);
  }
}

async function openedpassData(item) {
  docId.value = item.id;
  docName.value = item.name;
  opened.value = true;
}

async function editData(item) {
  name.value = item.name;
  specifications.value = item.specifications;
  price.value = item.price;
  fileme.value.push(item.image);
  editModal.value = true;
}

const addOrder = (item, quantity = 1) => {
  const firstFiveCharacters = uuid.value.slice(0, 5);
  const currentYear = new Date().getFullYear();
  const currentDate = new Date();
  const options = { month: "short", day: "numeric", year: "numeric" };
  const formattedDate = currentDate.toLocaleDateString("en-US", options);

  const existingOrder = orders.value.find((order) => order.itemId === item.id);
  if (existingOrder) {
    // Item already exists in orders, update the quantity
    existingOrder.order_quantity += quantity;
    return;
  }

  const newOrder = {
    itemId: item.id,
    userid: item.userid,
    orderId: "ORDER#" + currentYear + firstFiveCharacters,
    name: item.name,
    price: item.price,
    desc: item.description,
    category: item.category,
    image: item.image,
    orderDate: formattedDate,
    order_quantity: quantity,
    payment_status: "unpaid",
  };
  orders.value = [...orders.value, { ...newOrder, grand_total: grandTot }];
  console.log("ORDERS", orders.value);
};
async function placeOrder() {
  try {
    const docRef = await addDoc(collection(db, "orders"), {
      orders: orders.value, // Pass the orders value to the "customerorder" collection
    });
    const dialog = $q.dialog({
        title: 'Uploading...',
        dark: true,
        message: '0%',
        progress: {
          spinner: QSpinnerGears,
          color: 'amber'
        },
        persistent: true, // we want the user to not be able to close it
        ok: false // we want the user to not be able to close it
      })

      // we simulate some progress here...
      let percentage = 0
      const interval = setInterval(() => {
        percentage = Math.min(100, percentage + Math.floor(Math.random() * 22))

        // we update the dialog
        dialog.update({
          message: `${percentage}%`
        })

        // if we are done...
        if (percentage === 100) {
          clearInterval(interval)

          dialog.update({
            title: 'Done!',
            message: 'Upload completed successfully',
            progress: false,
            ok: true
          })
        }
      }, 300)
    orders.value = [];
    console.log("Document written with ID: ", docRef.id);
  } catch (e) {
    console.error("Error adding document: ", e);
  }
}
const totalAmount = computed(() => {
  grandTot.value = 0;
  for (const order of orders.value) {
    grandTot.value += order.price * order.order_quantity;
  }
  return grandTot.value;
});
const increaseQuantity = (order) => {
  order.order_quantity++;
};

const decreaseQuantity = (order) => {
  if (order.order_quantity > 1) {
    order.order_quantity--;
  }
};

// Place the order logic goes here
const cancelOrder = () => {
  orders.value = [];
};

async function confirmed() {
  opened.value = false;
  await deleteDoc(doc(db, "iam", docId.value));
}
onServerPrefetch(async () => {
  auth = getAuth();
  onAuthStateChanged(auth, (user) => {
    // uuid.value = user.uid
    // console.log(uuid.value)
    if (user) {
      isLoggedIn.value = true;
      console.log(isLoggedIn.value);
    } else {
      isLoggedIn.value = false;
      console.log(isLoggedIn.value);
    }
  });
});
var user = getAuth().currentUser;
var nameme, email, photoURL, uid;
uuid.value = user.uid;
if (user != null) {
  nameme = user.displayName;
  uid = user.uid;
}
watch(tab, (newTab, oldTab) => {
  console.log("tab", tab)
  console.log("tab", newTab)

  // Unsubscribe from the previous query (if any)
  if (unsubscribe.value) {
    unsubscribe.value();
  }

  // Set the initial category value to "starter" if newTab is falsy
  const category = newTab;
  if(category === "all"){
    q.value = query(
    collection(db, 'iam'),
    where('userid', '==', uuid.value),
  );
  }else{
    q.value = query(
    collection(db, 'iam'),
    where('userid', '==', uuid.value),
    where('category', '==', category),
  );

  }

  // Update the Firestore query with the new tab value

  // Subscribe to the new query and update data
  unsubscribe.value = onSnapshot(q.value, (snapshot) => {
    datassss.value = []; // Clear previous data

    snapshot.docChanges().forEach((change) => {
      var data = {
        ...change.doc.data(),
        ...{ id: change.doc.id },
      };
      if (change.type === 'added') {
        datassss.value.push(data);
      }
      if (change.type === 'modified') {
        console.log('Modified city: ', data);
      }
      if (change.type === 'removed') {
        const index = datassss.value.findIndex(
          (d) => d.name === change.doc.data().name
        );
        console.log(
          'Removed city: ',
          change.doc.data(),
          change.doc.id
        );
        datassss.value.splice(index, 1);
      }
    });
  });
});
const handleSignOut = () => {
  const auth = getAuth();
  signOut(auth)
    .then(() => {
      // Sign-out successful.
      router.push("/sign-in");
      console.log("Sign-out successful.");
    })
    .catch((error) => {
      router.push("/");
      console.log(error);
    });
};

function cancelEdit() {
  editModal.value = false;
}
onMounted(async () => {
  // Set the initial tab value to "starter"
  tab.value = "all";

  // Update the Firestore query with the initial tab value
  q.value = query(
    collection(db, "iam"),
    where("userid", "==", uuid.value),
  );

  // Subscribe to the query and update data
  unsubscribe.value = onSnapshot(q.value, (snapshot) => {
    datassss.value = []; // Clear previous data

    snapshot.docChanges().forEach((change) => {
      var data = {
        ...change.doc.data(),
        ...{ id: change.doc.id },
      };
      if (change.type === "added") {
        datassss.value.push(data);
      }
      if (change.type === "modified") {
        console.log("Modified city: ", data);
      }
      if (change.type === "removed") {
        const index = datassss.value.findIndex(
          (d) => d.name === change.doc.data().name
        );
        console.log(
          "Removed city: ",
          change.doc.data(),
          change.doc.id
        );
        datassss.value.splice(index, 1);
      }
    });
  });
});
</script>
<style>
  .hover-tab:hover {
    background-color: #d8e6e9 !important;
    border-radius: 20px;
  }
</style>
