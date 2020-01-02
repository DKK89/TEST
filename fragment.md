# Fragment

 TeamEverywhere Java Guide 

 **Table of Contents**

##  Fragment 

* Fragment : Activity 내의 어떤 동작 또는 사용자 인터페이스의 일부를 나타냄
* Activity는 하나의 창에 하나의 Activity 만 구현 가능, 반면에 Fragment는 여러 개의 프래그먼트를 하나의 액티비티에 결합하여 창이 여러 개인 UI를 빌드할 수 있다 
* 하나의 프래그먼트를 여러 액티비티에서 재사용 가능 
* 자체적인 수명 주기를 가지고, 자체 입력 이벤트를 수신하고, 액티비티 실행 중에 추가 및 삭제가 가능 
* 프래그먼트는 항상 액티비티 내에서 호스팅되어야 하며 해당 프래그먼트의 수명 주기는 호스트 액티비티의 수명 주기에 직접적으로 영향 \(ex&gt; 액티비티가 일시정지 되면 해당 액티비티 하위의 모든 프래그먼트도 일시 정지, 액티비티 소멸 시 프래그먼트도 모두 소멸\) 
* 프래그먼트 트랜잭션을 통해 액티비티가 관리하는 백 스택에도 추가\(BackButton 과 관련된 동작 가능\) 
  * 주로 추가할 때는 add\(\), replace\(\) 메소드를 통해 추가. 
  *  요소로 프래그먼트를 선언하거나 기존 ViewGroup에 추가하는 방법이 있음 

- 

```java
//Activity 
getSupportFragmentManager().beginTransaction().add(R.id.frameLayout, fragment).addToBackStack(null).commit();

//Fragment 
getFragmentManager().beginTransaction().add(R.id.frameLayout, fragment).addToBackStack(null).commit();
```

* activity xml에서 FrameLayout 선언필요 - 다른 뷰보다 위에 나타나야 하는 경우가 많기 때문에 activity xml의 최하단에 주로 선언 

```markup
<androidx.constraintlayout.widget.ConstraintLayout 
    android:layout_width="match_parent" 
    android:layout_height="match_parent"> 
    
    ...

    <FrameLayout
        android:id="@+id/frameLayout" 
        android:layout_width="match_parent" 
        android:layout_height="match_parent" /> 
        
</androidx.constraintlayout.widget.ConstraintLayout>
```

* 혹은 fragment를 직접 호출 가능 - 해당 뷰에 고정적으로 놔두고 변경이 필요하지 않은 경우 사용 

```markup
<?xml version="1.0" encoding="utf-8"?> 
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent" 
    android:layout_height="match_parent"> 
    
    <fragment 
        android:name="com.example.news.FirstFragment" 
        android:id="@+id/list" 
        android:layout_weight="1" 
        android:layout_width="0dp" 
        android:layout_height="match_parent" /> 
     
     <fragment 
        android:name="com.example.news.SecondFragment" 
        android:id="@+id/viewer" 
        android:layout_weight="2" 
        android:layout_width="0dp" 
        android:layout_height="match_parent" /> 
        
</LinearLayout>
```

![fragment\_lifecycle](https://user-images.githubusercontent.com/45971970/71537352-2608d880-295e-11ea-95db-b4d7d55ef1b7.png)

* 액티비티와 비슷한 콜백 메서드가 들어 있음을 알 수 있음. 예를 들어 onCreate\(\), onStart\(\), onPause\(\) 및 onStop\(\) 등
* 보통은 최소한 다음과 같은 수명 주기 메서드를 구현해야 함
  * onCreate\(\) : 프래그먼트를 생성할 때 시스템에서 호출. 구현 내에서 프래그먼트의 기본 구성 요소 중 프래그먼트가 일시정지되거나 중단되었다가 재개되었을 때 유지하고자 하는 것을 초기화
  * onCreateView\(\) : 프래그먼트가 자신의 사용자 인터페이스를 처음으로 그릴 때 호출. 프래그먼트에 맞는 UI를 그리려면 메서드에서 View를 반환해야 함 \(return view\). 이 메소드는 프래그먼트 레이아웃의 루트이며, 프래그먼트가 UI를 제공하지 않는 경우 null을 반환
  * onPause\(\) : 이 메소드를 호출하는 것은 사용자가 프래그먼트를 떠난다는 것을 나타내는 첫 번째 신호 \(다만 항상 프래그먼트가 소멸 중이라는 것을 의미하지는 않음\). 일반적으로 여기에서 현재 사용자 세션을 넘어서 지속되어야 하는 변경 사항을 커밋
* 사용자 인터페이스 추가 

  * 프래그먼트에 대해 레이아웃을 제공하려면 반드시 onCreateView\(\) 콜백 메서드를 구현해야 함 
  * onCreateView\(\)로부터 레이아웃을 반환하려면 이를 XML에서 정의된 레이아웃 리소스로부터 팽창 
  * inflate\(\) 메소드 : inflate\(팽창\)시키고자 하는 레이아웃의 리소스 ID 팽창된 레이아웃의 상위가 될 ViewGroup. container를 전달해야 \(주로 상위 액티비티의 FrameLayout\) 시스템이 레이아웃 매개변수를 팽창된 레이아웃의 루트 뷰에 적용할 수 있으므로 중요 팽창된 레이아웃이 팽창 중에 ViewGroup\(두 번째 매개변수\)에 첨부되어야 하는지를 나타내는 부울 값. \(대부분 false : 시스템이 이미 팽창된 레이아웃을 container 안에 삽입하므로. true 전달 시 최종 레이아웃에 중복된 뷰 그룹을 생성\) 

  `java`  참고: [https://developer.android.com/guide/components/fragments?hl=ko\#java](https://developer.android.com/guide/components/fragments?hl=ko#java)

```java
public static class ExampleFragment extends Fragment { 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        // Inflate the layout for this fragment 
        return inflater.inflate(R.layout.example_fragment, container, false);
    }
} 

public static class ExampleFragment extends Fragment { 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) { 
    // Inflate the layout for this fragment 
        View view = inflater.inflate(R.layout.fragment_example, container, false); 
        view.setOnTouchListener(new View.OnTouchListener() { 
            @Override 
            public boolean onTouch(View v, MotionEvent event) { 
                return false;
            } 
        });
        
        return view; 
    } 
}
```

