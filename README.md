## 1. Đặt tên function

- Đối với các function dùng để gán vào các event dạng như `onClick`, `onFocus`, ... thì tên function nên bắt đầu bắng tứ khóa `handle`. Ví dụ:

  ```js
  <button onClick={handleSubmitForm}>Submit</button>
  ```

- Đối với các function không dùng để gán vào event dạng nói trên thì không nên đặt tên function bắt đầu bằng từ khóa `handle`



## 2. Thư mục `utils/`

- Thư mục này sẽ chủ yếu chứa các function được dùng chung ở nhiều chỗ (function có thể tái sử dụng), vì thế nên chú ý có nên cho function vào đây không. 
- Khi thêm một function vào folder cần chú ý:
  - Trường hợp function là độc lập không nên tạo một file mới trong folder `utils/` chỉ chứa duy nhất một function mà hãy thêm nó vào một file chung như `helpers.js`
  - Trường hợp đưa một nhóm function vào folder `utils/` thì hãy tạo mới một file mới trong folder này để chứa nhóm function nói trên



## 3. Eslint Rule

- Không nên xóa hoặc disable các rule trong eslint trừ trường hợp bắt buộc không làm khác được và nên trao đổi với mọi người trước khi xóa hoặc disable



## 4. Redux

- Các function dùng để cập nhật state trong reducer làm vượt quá số lượng max line (180) thì nên tách nhóm function đó ra một folder riêng có thể thể đặt tên là `mutators/` và bên trong đó tạo file chứa nhóm function này và import lại vào trong file `reducer.js` để tránh trường hợp sau này code sẽ quá dài và khó trong việc sửa chữa. Ví dụ:

  ```
  cv/
  	- mutators/
  		+ layout.js
  	- actions.js
  	- index.js
  	- reducer.js
  	- types.js
  ```



- Trong các file như `types.js`,  `actions.js`, `reducer.js` ở trong  phần khai báo function hoặc export có thể nhóm các function thuộc chung một `modules` thành một nhóm và viết cách dòng với các function khác. Ví dụ:

```js
//=============== ACTIONS ===============//
// Nhóm function liên quan đến lấy dữ liệu từ API
const load = createAction(types.LOAD_CV);
const loadSuccess = createAction(types.LOAD_CV_SUCCESS);
const loadFail = createAction(types.LOAD_CV_FAIL);
const storeData = createAction(types.STORE_CV);

// Nhóm function liên quan đến thay đổi layout
const addRow = createAction(types.ADD_ROW, payload => payload);
const updateRow = createAction(types.UPDATE_ROW, payload => payload);
const deleteRow = createAction(types.DELETE_ROW, payload => payload);
const moveRow = createAction(types.MOVE_ROW, payload => payload);


export const actions = {
    load,
    loadSuccess,
    loadFail,
    storeData,

    addRow,
    updateRow,
    deleteRow,
    moveRow,
};
```



## 5. String

- Khi sử dụng các hàm của string trong js như: `toUpperCase()`, `split()`, ... thì không nên dùng trực tiếp trên biến lấy về từ API như:

  ```js
  data.get('name').toUpperCase();
  ```

  Vì trong trường hợp `data.get('name')` bị null hoặc undefined sẽ dẫn đến toàn bộ app bị crash

- Thay vào đó nên viết một function dùng chung trong helpers dạng như:

  ```js
  const toUpperCase = (str) => {
      if (str && typeof str === 'string') {
          return str.toUpperCase();
      }
      return '';
  };
  
  => toUpperCase(data.get('name'));
  ```

  
