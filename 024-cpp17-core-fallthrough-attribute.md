## [[fallthrough]]属性

[[fallthrough]]属性はswitch文の中のcaseラベルを突き抜けるというヒントを出すのに使える。

switch文では対応するcaseラベルに処理が移る。通常、以下のように書く。


~~~cpp
void f( int x )
{
    swtich ( x )
    {
    case 0 :
        // 処理0
        break ;
    case 1 :
        // 処理1
        break ;
    case 2 :
        // 処理2
        break ;
    default :
        // xがいずれでもない場合の処理
        break ;
    }
}
~~~

この例を以下のように書くと

~~~c++
case 1 :
    // 処理1
case 2 :
    // 処理2
    break ;
~~~


xが1の時は処理1を実行した後に、処理2も実行される。switch文を書くときはこのような誤りを書いてしまうことがある。そのため、賢いC++コンパイラーはswitch文のcaseラベルでbreak文やreturn文などで処理が終わらず、次のcaseラベルやdefaultラベルに処理に突き抜けるコードを発見すると、警告メッセージを出す。


しかし、プログラマーの意図がまさに突き抜けて処理して欲しい場合、警告メッセージは誤った警告となってしまう。そのような警告メッセージを抑制するため、またコード中に処理が突き抜けるという意図をわかりやすく記述するために、[[fallthrough]]属性が追加された。

~~~c++
case 1 :
    // 処理1
    [[fallthrough]]
case 2 :
    // 処理2
    break ;
~~~

[[fallthrough]]属性を書くと、C++コンパイラーは処理がその先に突き抜けることがわかるので、誤った警告メッセージを抑制できる。また、他人がコードを読むときに意図が明らかになる。


機能テストマクロは__has_cpp_attribute(fallthrough), 値は201603。