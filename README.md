```
// ①〜⑨のコメントを参考に、Todoリストアプリにおける、Todo追加の機能を実装しましょう
// ⑩〜⑬のコメントを参考に、Todo削除の機能も実装しましょう
// 14 〜 24のコメントを参考に、Todo編集の機能も実装しましょう

import { useState } from 'react'
import './App.css'

function App() {
  // ① 入力内容を格納する変数（useState）を定義
  const [text, setText] = useState("")

  // ④ 複数のTodoを格納する配列（リスト）（useState）を定義
  const [todos, setTodos] = useState([])

  // 16 どのTodoを編集するのかを格納する変数（useState）を定義
  // => なぜ削除ボタンでは、useStateを定義しなかったのに、編集ボタンではuseStateを定義する必要があるのか？
  // => 編集中のTodoの見た目を変える必要がある（入力フォーム + 保存ボタン）ため、どのTodoを編集しているのかを管理する必要がある
  const [editIndex, setEditIndex] = useState(null)

  // 19 編集中の入力内容を格納する変数（useState）を定義
  const [editText, setEditText] = useState("")

  // ③ 入力内容を変数に格納する関数を定義
  const handleInput = (e) => {
    setText(e.target.value)
  }

  // 20 編集の入力内容を変数に格納する関数を定義
  const handleEditInput = (e) => {
    setEditText(e.target.value)
  }

  // ⑥ 入力内容が空でない場合、その入力内容を配列に追加する関数を定義
  //  （入力内容が空の場合はアラートを表示）
  // ⑧ 追加後、入力フォームの中身を空にする
  const handleAdd = () => {
    if (text.trim() !== "") {
      setTodos([...todos, text])
    } else {
      alert("タスクを入力してください")
    }
    setText("")
  }

  // ⑬ Todoを削除する関数を定義（filterメソッドを使おう）
  const handleDelete = (index) => {
    const newTodos = todos.filter((todo, i) => i !== index)
    setTodos(newTodos)
  }

  // 22 編集ボタンを押した時に動く関数を定義
  //    (入力フォームと保存ボタンを表示させ、入力フォームには初期値として元のTodoの内容を入れておく)
  const handleEdit = (index) => {
    setEditIndex(index)
    setEditText(todos[index])
  }

  // 23 入力内容が空でない場合、配列内のTodoが変更されるようにする（mapを使おう）
  //  （入力内容が空の場合はアラートを表示）
  // 24 入力フォーム＋保存ボタンという見た目を、Todo＋編集ボタン＋削除ボタンという見た目に戻す
  const handleSave = () => {
    if (editText.trim() !== "") {
      const newTodos = todos.map((todo, i) => i === editIndex ? editText : todo)
      setTodos(newTodos)
      setEditIndex(null)
    } else {
      alert("タスクを入力してください")
    }
  }

  return (
    <div className='container'>
      <h1>Todoリスト</h1>
      <div className='input-area'>
        <input
          placeholder='やることを入力...'
          // ② 入力内容が変更された時の処理を定義
          onChange={handleInput}

          // ⑦ 入力フォームの中身を変数に格納
          value={text}
        />
        <button
          // ⑤ 追加ボタンが押された時の処理を定義
          onClick={handleAdd}
        >
          追加
        </button>
      </div>
      <ul>
        {/* ⑨ 配列に格納された複数のTodoを１つずつ取り出して表示（mapを使おう） */}
        {/* ⑩ 削除ボタンを追加（classNameを指定する） */}
        {/* ⑪ CSS を追加する（App.cssへ） */}
        {/* ⑫ 削除ボタンが押された時の処理を定義（どのTodoを削除するのか、という情報を定義した関数に渡す） */}
        {todos.map((todo, index) => (
          <li>
            {/* 17 三項演算子を使って編集中のTodoのみ、見た目を変更しよう（入力フォーム + 保存ボタン） */}
            {editIndex === index ? (
              <>
                {/* 18 編集の入力内容が変更された時・保存ボタンが押された時の処理を定義 */}
                <input
                  value={editText}
                  onChange={handleEditInput}
                />
                <button onClick={handleSave}>保存</button>
              </>
            ) : (
              <>
                {todo}
                {/* 14 編集ボタンを追加 */}
                {/* 15 編集ボタンと削除ボタンとの間隔を開けよう（ CSSを追加 ） */}
                <div className='buttons'>
                  {/* 21 編集ボタンが押された時の処理を定義 */}
                  <button onClick={() => handleEdit(index)}>編集</button>
                  <button className='delete-button' onClick={() => handleDelete(index)}>削除</button>
                </div>
              </>
            )}
          </li>
        ))}
      </ul>
    </div>
  )
}

export default App
```