```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';


// class Square extends React.Component {
//     render() {
//         return (
//             <button className="square">
//                 {/* TODO */}
//             </button>
//         );
//     }
// }
//
// class Board extends React.Component {
//     renderSquare(i) {
//         return <Square />;
//     }
//
//     render() {
//         const status = 'Next player: X';
//
//         return (
//             <div>
//                 <div className="status">{status}</div>
//                 <div className="board-row">
//                     {this.renderSquare(0)}
//                     {this.renderSquare(1)}
//                     {this.renderSquare(2)}
//                 </div>
//                 <div className="board-row">
//                     {this.renderSquare(3)}
//                     {this.renderSquare(4)}
//                     {this.renderSquare(5)}
//                 </div>
//                 <div className="board-row">
//                     {this.renderSquare(6)}
//                     {this.renderSquare(7)}
//                     {this.renderSquare(8)}
//                 </div>
//             </div>
//         );
//     }
// }
//
// class Game extends React.Component {
//     render() {
//         return (
//             <div className="game">
//                 <div className="game-board">
//                     <Board />
//                 </div>
//                 <div className="game-info">
//                     <div>{/* status */}</div>
//                     <ol>{/* TODO */}</ol>
//                 </div>
//             </div>
//         );
//     }
// }
//
// // ========================================
//
// ReactDOM.render(
//     <Game />,
//     document.getElementById('root')
// );




// Board の renderSquare メソッド内で、props として value という名前の値を Square に渡すようにコードを変更
class Board extends React.Component {
    // Board にコンストラクタを追加し、初期 state に 9 個の null が 9 個のマス目に対応する 9 個の null 値をセットします。
    constructor(props) {
        super(props);
        this.state = {
            squares: Array(9).fill(null),
            // デフォルトでは、先手を “X” にします。Board のコンストラクタで state の初期値を変えればこのデフォルト値は変更可能です。
            xIsNext: true,
        };
    }


    // squares を直接変更する代わりに、.slice() を呼んで配列のコピーを作成していることに注意してください
    // なぜ配列のコピーをするのか？それは、データの値をミューテート（書き換え）するためと、値の上書きをするため
    handleClick(i) {
        const squares = this.state.squares.slice();

        // Board の handleClick を書き換えて、ゲームの決着が既についている場合やクリックされたマス目が既に埋まっている場合に早期に return するようにします。
        if (calculateWinner(squares) || squares[i]) {
            return;
        }

        // squares[i] = 'X';
        // xIsNextを真偽値にしている
        squares[i] = this.state.xIsNext ? 'X' : 'O';

        // this.setState({squares: squares});
        // xIsNext の値を反転させて、“X” 側と “O” 側が交互に着手できるようになります。
        // クリックするとXか0かが入力される。が、初期値をXとしているので、0を入力するには2回押さないといけない。

        this.setState({
            squares: squares,
            xIsNext: !this.state.xIsNext,
        });
    }

    // renderSquareというメソッドを使用し、iという引数を宣言
    renderSquare(i) {
        // Squareにvalueという名前の値をSquareに渡す
        // return <Square value={i}/>;

        // Boardでsquaresを設定したので、そのstateをreturnしてあげる
        // Board を書き換えて、それぞれの個別の Square に現在の値（'X'、'O' または null）を伝えるようにします
        // Board から Square に関数を渡すことにして、マス目がクリックされた時に Square にその関数を呼んでもらうようにしましょう。
        return (
            // Squareクラスにpropsを渡している（props = value,onClick）
            <Square
                value={this.state.squares[i]}
                onClick={() => this.handleClick(i)}
            />
        );
    }

    // render() {
    //     // const status = 'Next player: X';
    //     // どちらのプレーヤの手番なのかを表示する
    //     const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');


    // Board の render 関数内で calculateWinner(squares) を呼び出して、いずれかのプレーヤが勝利したかどうか判定します。決着がついた場合は “Winner: X” あるいは “Winner: O” のようなテキストを表示するとよいでしょう
    render() {
        const winner = calculateWinner(this.state.squares);
        let status;
        if (winner) {
            status = 'Winner: ' + winner;
        } else {
            status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
        }

        return (
            <div>
                <div className="status">{status}</div>
                <div className="board-row">
                    {this.renderSquare(0)}
                    {this.renderSquare(1)}
                    {this.renderSquare(2)}
                </div>
                <div className="board-row">
                    {this.renderSquare(3)}
                    {this.renderSquare(4)}
                    {this.renderSquare(5)}
                </div>
                <div className="board-row">
                    {this.renderSquare(6)}
                    {this.renderSquare(7)}
                    {this.renderSquare(8)}
                </div>
            </div>
        );
    }
}

// // quare の render メソッドで、渡された値を表示するように、{/* TODO */} を {this.props.value}
// class Square extends React.Component {
//     // React コンポーネントはコンストラクタで this.state を設定することで、状態を持つことができるようになります
//     // クラスにコンストラクタを追加して state を初期化します
//     constructor(props) {
//         super(props);
//         this.state = {
//             value: null,
//         };
//     }
//
//     // renderは出力
//     render() {
//         return (
//             /*<button className="square" onClick={function () { alert('click'); }}>*/
//             /*<button className="square" onClick={() => alert('click')}>*/
//             /*/!*Square の render メソッドで、渡された値を表示する。propsは状態を保存してくれるので、異なるクラスでも発揮される*!/*/
//             /*{this.props.value}*/
//             /*</button>*/
//
//             // this.props.value -> this.state.valueに変更する
//             // onClick={() => this.setState({value: 'X'})}にしてStateを継承する準備
//             // 空のstateをXにする作業
//             <button
//                 className="square"
//                 // onClick={() => this.setState({value: 'X'})}
//                 onClick={() => this.props.onClick()}
//             >
//                 {/*{this.state.value}*/}
//
//                 {/*Boardのpropsを渡している*/}
//                 {this.props.value}
//             </button>
//
//         );
//     }
// }

function Square(props) {
    return (
        // <button className="square" onClick={() => this.props.onClick()}>と同意
        <button className="square" onClick={props.onClick}>
            {props.value}
        </button>
    );
}

class Game extends React.Component {
    render() {
        return (
            <div className="game">
                <div className="game-board">
                    <Board />
                </div>
                <div className="game-info">
                    <div>{/* status */}</div>
                    <ol>{/* TODO */}</ol>
                </div>
            </div>
        );
    }
}

function calculateWinner(squares) {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
    }
    return null;
}

// ========================================

ReactDOM.render(
    <Game />,
    document.getElementById('root')
);



// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```
