<?php
// ・データベース名
$dsn = 'mysql:dbname=データベース名;host=localhost';
// ・ユーザー名
$user = 'ユーザー名';
// ・パスワード：
$password = 'パスワード';
$pdo = new PDO($dsn, $user, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_WARNING));

$up_name = '';
$up_comment = '';
$up_id = '';
$up_pass = '';

// 4-2 テーブルを作る
 
$sql = "CREATE TABLE IF NOT EXISTS tbtest_2"
        ." ("
        . "id INT AUTO_INCREMENT PRIMARY KEY,"
        . "name CHAR(32),"
        . "comment TEXT,"
        . "password CHAR(32),"
        . "date CHAR(32)"
        .");";
 $stmt = $pdo->query($sql);

 //1-25参考
 // もしフォームから「name」が送信されてきた場合のみ、書き込みを実行する
if (isset($_POST['name']) && $_POST['name'] !== '' && isset($_POST['submit'])) {
    $name = $_POST['name'];
    $comment = $_POST['comment'];
    $date = date('Y/m/d H:i:s');
    $pasword = $_POST['password'];
    
    
    if ( isset($_POST['edit_id']) && $_POST['edit_id'] !== '' ) {
        
        //編集番号がある場合は上書き修正
        $edit_id = $_POST['edit_id'];
        $sql = 'UPDATE tbtest_2 SET name=:name, comment=:comment, password=:password, date=:date WHERE id=:id';
        $stmt = $pdo -> prepare ( $sql ) ; 
        $stmt -> bindParam ( ':id' , $edit_id , PDO :: PARAM_INT ) ; 
        $stmt -> bindParam ( ':name' , $name , PDO :: PARAM_STR ) ; 
        $stmt -> bindParam ( ':comment' , $comment , PDO :: PARAM_STR ) ; 
        $stmt -> bindParam ( ':password' , $pasword , PDO :: PARAM_STR ) ;
        $stmt -> bindParam ( ':date' , $date , PDO :: PARAM_STR ) ;
        $stmt -> execute ( ) ; 
        
    } else {
        $sql = "INSERT INTO tbtest_2 (name, comment, password, date) VALUES (:name, :comment, :password, :date)";
        $stmt = $pdo->prepare($sql);
        $stmt->bindParam(':name', $name, PDO::PARAM_STR);
        $stmt->bindParam(':comment', $comment, PDO::PARAM_STR);
        $stmt->bindParam(':password', $pasword, PDO::PARAM_STR);
        $stmt->bindParam(':date', $date, PDO::PARAM_STR);
        $stmt->execute();
        
    }
}

// 編集ボタン（up）が押されたとき
if (isset($_POST['up']) && isset($_POST['nam2']) && isset ( $_POST [ 'nam2' ]) && $_POST['nam2'] !== '' &&  $_POST['password2'] !== '') {
    $up_id = $_POST['nam2'];
    $up_pass = $_POST [ 'password2' ] ;

$sql = 'SELECT * FROM tbtest_2 WHERE id = :id';
    $stmt = $pdo->prepare($sql);
    $stmt->bindParam(':id', $up_id, PDO::PARAM_INT);
    $stmt->execute();
    $up_record = $stmt->fetch(); // 1件だけ取得

    if ($up_record) {
        if ( $up_record['password'] === $up_pass ) {
            // パスワードが一致した場合のみ、フォームに表示する変数に代入する
            $editName = $up_record['name'] ; 
            $editComment = $up_record['comment'] ;
            $editpassword = $up_record['password'] ;
            $editId = $up_id;
        } else {
            echo "<p style='color: red;'>パスワードが違います。</p>";
        }
        
    }
    
}


// 削除ボタン（der）が押されたとき
if ( isset ( $_POST [ 'der' ]) && isset ( $_POST [ 'nam1' ]) && $_POST [ 'nam1' ] !== '' && isset ( $_POST [ 'password1' ]) && $_POST [ 'password1' ] !== '' ) { 
    $del_id = $_POST [ 'nam1' ] ; 
    $del_pass = $_POST [ 'password1' ] ; 
    
    // 1. まずデータベースから、削除したい番号のデータを取ってくる
    $sql = 'SELECT * FROM tbtest_2 WHERE id = :id' ; 
    $stmt = $pdo -> prepare ( $sql ) ; 
    $stmt -> bindParam ( ':id' , $del_id , PDO :: PARAM_INT ) ; 
    $stmt -> execute ( ) ; 
    $del_record = $stmt -> fetch ( ) ; // ここで $del_record を作ります！
    
    if ( $del_record ) { 
        // 2. データベースのパスワードと、入力されたパスワードが一致するかチェック
        if ( $del_record [ 'password' ] === $del_pass ) { 
            // 3. 一致したら正しいテーブル名（tbtest_2）で削除を実行
            $sql = 'DELETE FROM tbtest_2 WHERE id = :id' ; 
            $stmt = $pdo -> prepare ( $sql ) ; 
            $stmt -> bindParam ( ':id' , $del_id , PDO :: PARAM_INT ) ; 
            $stmt -> execute ( ) ; 
        } else { 
            echo "<p style='color: red;'>パスワードが違います。</p>" ; 
        } 
    } 
}


$str = 'SELECT * FROM tbtest_2 ORDER BY id';
$stmt = $pdo->query($str);
$results = $stmt->fetchAll();

 
?>

<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title></title>
</head>
<body>
  <form action="" method="post">
      <input type="hidden" name="edit_id" placeholder="番号" value="<?php if(isset($editId)){ echo htmlspecialchars($editId, ENT_QUOTES, 'UTF-8'); } ?>">
      <input type="text" name="name" placeholder="名前" value="<?php if(isset($editName)){ echo htmlspecialchars($editName, ENT_QUOTES, 'UTF-8'); } ?>">
      <input type="text" name="comment" placeholder="コメント" value="<?php if(isset($editComment)){ echo htmlspecialchars($editComment, ENT_QUOTES, 'UTF-8'); } ?>">
      <input type="password" name="password" placeholder="パスワード" value="<?php if(isset($editpassword)){ echo htmlspecialchars($editpassword, ENT_QUOTES, 'UTF-8'); } ?>">
      <input type="submit" name="submit">
      <br />
      <input type="text" name="nam1" placeholder="番号">
      <input type="password" name="password1" placeholder="パスワード">
      <input type="submit" name="der" value="削除">
      <br />
      <input type="text" name="nam2" placeholder="番号">
      <input type="password" name="password2" placeholder="パスワード">
      <input type="submit" name="up" value="編集">
      <br />
      
      <?php
// ループして、取得したデータを表示
foreach ($results as $row) {
  // $row の中にはテーブルのカラム名が入る
  echo $row['id'].',';
  echo $row['name'].',';
  echo $row['comment'].',';
  echo $row['date'].'<br />';
  echo "<hr>";
}
  ?>
  </form>
</body>
</html>
