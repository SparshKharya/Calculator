 <html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Calculator</title>
    <link rel="stylesheet" href="style.css">
	<style>
		@import url('https://fonts.googleapis.com/css?family=Roboto+Mono:300,400,500,700');

body {
    margin: 0;
    font-size: 18px;
    font-family: "Roboto Mono";
    background: lightskyblue;
}

#calculator {
    margin: auto;
    margin-top: 10%;
    width: 400px;
    border-radius: 10px;
    box-shadow: 0 0 3em #cdcdcd;
}

header {
    padding: 20px;
    background: orange;
    color: white;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    text-align: right;
}

header #detail {
    font-size: 1em;
}

header #result {
    font-size: 2em;
}

header span {
    margin-left: .5em;
    float: left;
}

main {
    height: 60%;
    background: white;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
    overflow: auto;
}

main button {
    font-size: 1.2em;
    font-family: "Roboto Mono";
    float: left;
    width: 25%;
    border: 0;
    padding: 1em;
    cursor: pointer;
    background: white;
}

main button:hover {
    background: #cdcdcd;
}

main button:focus {
    outline: none;
}

main button:active {
    box-shadow: 0 0 10px #cdcdcd;
}
		footer{
    background: #111;
    padding: 15px 23px;
    color: #fff;
    text-align: center;
}
footer span a{
    color:yellow;
    text-decoration: none;
}
footer span a:hover{
    text-decoration: underline;
}

	</style>
</head>
<body>
    <div id="calculator">
        <header>
            <label id="detail"></label>
            <div id="result">
                <span>=</span>
                <div id="result-value">0</div>
            </div>
        </header>
        <main id="keys">
            <button>C</button>
            <button>+/-</button>
            <button>%</button>
            <button>/</button>
            <button>7</button>
            <button>8</button>
            <button>9</button>
            <button>*</button>
            <button>4</button>
            <button>5</button>
            <button>6</button>
            <button>-</button>
            <button>1</button>
            <button>2</button>
            <button>3</button>
            <button>+</button>
            <button>0</button>
            <button>.</button>
            <button>del</button>
            <button>=</button>
        </main>
    </div>

    <script src="calculation.js"></script>
	<script>
		window.onload = function() {
    // Declare all initiliation variables
    var keys         = document.getElementsByTagName('button'),
        operators    = ['/', '*', '-', '+', '%'],
        lastOperator = '',
        decimalAdded = false;
    
    // Loop all key
    for (var i = 0; i < keys.length; i++) {

        // Add click event listener to all key
        keys[i].onclick = function() {

            // Add initial variables
            var keyValue    = this.innerHTML,
                detail      = document.getElementById('detail'),
                detailValue = detail.innerHTML,
                lastChar    = detailValue[detailValue.length - 1],
                result      = document.getElementById('result-value');

            // Use switch case for different function of keys
            switch (keyValue) {
                // Case for clearing the calculator
                case 'C':
                    result.innerHTML = '0';
                    detail.innerHTML = '';
                    break;
                // Show the result for calculation
                case '=':
                    if (detail.innerHTML != '') {
                        result.innerHTML = eval(detailValue);
                        decimalAdded = false;
                    }
                    break;
                // Case for arithmatic operator
                case '/':
                case '*':
                case '-':
                case '+':
                case '%':
                    if (detailValue != '' && operators.indexOf(lastChar) == -1) {
                        detail.innerHTML += keyValue;
                    } else {
                        detail.innerHTML = detail.innerHTML.replace(/.$/, keyValue);
                    }

                    decimalAdded = false;
                    lastOperator = keyValue;
                    break;
                // Case for delete char
                case 'del':
                    if (lastChar == '.') {
                        decimalAdded = false;   
                    }

                    detail.innerHTML = detail.innerHTML.replace(/.$/, '');
                    break;
                // Case for add period
                case '.':
                    if ( ! decimalAdded) {
                        detail.innerHTML += keyValue;
                        decimalAdded = true;
                    }
                    break;
                // Case for signing minus/plus to the last calculation
                case '+/-':
                    if (detailValue != '' && operators.indexOf(lastChar) == -1) {
                        if (lastOperator == '') {
                            if (detailValue == Math.abs(detailValue)) {
                                detail.innerHTML = -(detailValue);
                            } else {
                                detail.innerHTML = Math.abs(eval(detailValue));
                            }
                        } else {
                            var array     = detail.innerHTML.split(lastOperator),
                                lastIndex = array.length - 1,
                                newDetail = '',
                                oldDetail = '';

                            if (array[lastIndex] == Math.abs(array[lastIndex])) {
                                newDetail = '(' + -(array[lastIndex]) + ')';
                            } else {
                                newDetail = Math.abs(eval(array[lastIndex]));
                            }

                            for (var i = 0; i < lastIndex; i++) {
                                oldDetail += array[i] + lastOperator;
                            }

                            detail.innerHTML = oldDetail + newDetail;
                        }
                    }
                    break;
                // Beside of that, just displaying the key value to the calculation
                // This is used for number
                default:
                    detail.innerHTML += keyValue;
                    break;
            }
        }
    }
}
		</script>
	<footer>
        <span>Created By <a href="#">SparshKharya</a> | <span class="far fa-copyright"></span> 2021 All rights reserved.</span>
    </footer>
</body>
</html>
