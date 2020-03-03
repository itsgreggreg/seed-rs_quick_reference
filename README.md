# Seed rs Quick Reference

# Debugging
## Logging to the console
 - the `log!` macro, can take multiple arguments
```rust
log!("hello", 5);
```
will log to the browser's console:
```
"hello"
5
```

# LocalStorage
```rust
// Use the modules:
use Seed::{
    browser::service::storage::{self, Storage},
}

// Create a property in your model to hold the storage object
struct Model {
   local_storage: Storage
}

// Create a storage key that your app uses to identify its storage object
const STORAGE_KEY: &str = "my-storage-key";

// Initialize local storage
fn after_mount(_: Url, _: &mut impl Orders<Msg>) -> AfterMount<Model> {
    let local_storage = storage::get_storage().expect("get `LocalStorage`");

    AfterMount::new(Model {
        local_storage: local_storage ,
    })
}

// Be sure to have an after mount step
#[wasm_bindgen(start)]
pub fn render() {
    App::builder(update, view)
        .after_mount(after_mount)
        .build_and_start();
}

// getting
let data: usize = storage::load_data(&local_storage, STORAGE_KEY).unwrap_or(5);

// setting
storage::store_data(&model.local_storage, STORAGE_KEY, &10);


```
