# Timer
- [TimersDashboard](#)
  - [ToggleableTimerForm = X / TimerForm](#)
  - [EditableTimerList](#)
    - [EditableTimer](#)
      - [Timer](#)
      - [TimerForm](#)
      

### Step 6: Invert data flow (event):

- __TimerForm__ :
Nhận 2 event như là props để xử lí các event. Cha của TimerForm chịu trách nhiệm cho việc cung cấp những hàm này.
  - When the form is submitted (creating or updating timer): `props.onFormSubmit()`
  - When the "Cacel" button is clicked (close the form): `props.onFormClose()`
  
  Inside __TimerForm__:
  - *Inside* render() use:
    - `onClick={this.handleSubmit}` là nút Submit
    - `onClick={this.props.onFormClose}` là nút Close
  - *Ouside* render() define:
    - `handleSubmit = () => {call thís.props.onFormSubmit(id: this.props.id, title: this.state.id, project: this.state.project)}`
    
    Việc ta phải sử dụng thêm một hàm handleSubmit mà không dùng luôn `thís.props.onFormSubmit()` từ component cha là vì
    hàm `this.props.onFormSubmit` sử dụng dữ liệu của state của hàm TimerForm này chứ không phải từ state của hàm cha cho nên
    để cho bên trong render của component TimerForm không phải truyền nhiều tham số ta khai báo thêm 1 hàm handleSubmit cho
    cho componnet TimerForm.
    
```JAVASCRIPT
handleSubmit = () => {
  this.props.onFormSubmit({
    id: this.props.id,
    title: this.state.title,
    project: this.state.project,
  });
};
```
    
- __ToggleableTimeForm (use TimerForm)__

  Submit event từ `TimerForm` nổi lên (bubble up) trong hệ thống phân cấp component (component hierarchy).
  Ta cần pass 2 prop-functions xuống TimerForm đó là `onFormClose()` và `onFormSubmit()`.
  - *Inside* render() add 2 props-function for TimerForm:
    - `onFormSubmit={this.handleFormSubmit}`
    - `onFormClose={this.handleFormClose}`
  - *Ouside* render() define:
    - `handleFormClose = () => {this.setSate({ isOpen: false })` dùng để đóng
    - `handleFormSubmit = (timer) => {this.props.onFormSubmit(timer); thís.setState({ isOpen: false });}
     gọi 1 hàm onFormSubmit từ component cha của ToggleableTimeForm là TimersDashboard, bởi vì chỉ có TimersDashboard mới
     đang lưu giữ các timer ở trong state của nó,và việc truyền id, title và project ở trên vào hàm onFormSubmit sẽ làm update
     state ở trong Dashboard, ta phải bubble các thay đổi mới từ việc submit lên tận Dashboard để Dashboard lưu vào state tổng.
     Sau khi các hàm onFormSubmit() và handleFormSubmit() được thực thi thì gọi setState isOpen: false cho TimerForm để đóng form lại
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
