<template>
  <div>
    <div align="right">
      <q-btn icon="add" label="Add Product" @click="addModal()" flat dense>
      </q-btn>
    </div>

    <div hidden align="right">
      <q-btn hidden @click="handleSignOut" v-if="isLoggedIn">log out</q-btn>
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
              :rules="[(val) => !!val || 'Please enter account  Name']"
              v-model="name"
            >
            </q-input>

            <!-- <q-input

              v-model="description"
              label="Description"
            >
            </q-input> -->

            <q-input v-model="specifications" label="Specifications"> </q-input>
            <q-input v-model.number="price" label="Price"> </q-input>
            <!-- <q-input

              v-model.number="pricebefore"
              label="Price Before"
            >
            </q-input> -->
            <q-input v-model="brand" label="Category"> </q-input>
            <!-- <q-rating
              v-model="ratings"
              max="5"
              size="3.5em"
              color="yellow"
              icon="star_border"
              icon-selected="star"
              icon-half="star_half"
              no-dimming
            /> -->
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
            <!-- <q-file
              filled
              bottom-slots
              v-model="fileme"
              label="Product View Image"
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

              <template v-slot:hint> Field hint </template>
            </q-file> -->
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
      <div class="q-pa-md row items-start">
        <q-card
          style="background-color: #d8e6e9 !important"
          class="my-card q-ma-md"
          :key="item.id"
          v-for="item in datassss"
          @mouseover="targetEl = true"
        >
          <q-img width="300px" height="250px" :src="item.image">
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
                item.brand
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
      <!-- edit item -->
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
        <q-card class="my-card bg-purple text-white">
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
import {
  onMounted,
  ref,
  getCurrentInstance,
  onBeforeMount,
  onServerPrefetch,
} from "vue";
import { useRouter } from "vue-router";
import { useQuasar } from "quasar";
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
const brand = ref("");
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
      brand: brand.value,
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

async function saveEdit() {}

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
console.log(user.uid);
uuid.value = user.uid;
if (user != null) {
  nameme = user.displayName;
  uid = user.uid;
}
console.log("yow", uuid.value);
const q = query(collection(db, "iam"), where("userid", "==", uuid.value));
const unsubscribe = onSnapshot(q, (snapshot) => {
  snapshot.docChanges().forEach((change) => {
    console.log("doc.data()", change.doc.data());
    // datassss.value =change
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
      console.log("Removed city: ", change.doc.data(), change.doc.id);
      datassss.value.splice(index, 1);
    }
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
</script>
