# 双向链表


![image-20231220211716208](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336751.png)

### 1、双向链表的增删改查

#### 1.1Java的实现思路

分析 双向链表的遍历，添加，修改，删除的操作思路===》代码实现

1. **遍历** 方和 单链表一样，只是可以向前，也可以向后查找

   ![image-20231220212515214](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336686.png)

   ```java
       //遍历双向链表
       public void list() {
           // 判断链表是否为空
           if (head.next == null) {
               System.out.println("链表为空");
               return;
           }
           // 因为头节点，不能动，因此我们需要一个辅助变量来遍历
           HeroNode2 temp = head.next;
           while (true) {
           // 判断是否到链表最后
               if (temp == null) {
                   break;
               }
               // 输出节点的信息
               System.out.println(temp);
               // 将temp后移， 一定小心
               temp = temp.next;
           }
       }
   ```

   

2. **添加** (默认添加到双向链表的最后)                                                       

   (1) 先找到双向链表的最后这个节点

   (2) temp.next = newHeroNode

   (3) newHeroNode.pre = temp;

   ![image-20231220212623249](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336694.png)

   ```java
     //在双向链表的尾部添加新的节点
       public void add(HeroNode2 heroNode2){
           HeroNode2 temp = this.head;
           //找到双向链表的尾部
           while (true){
               if(temp.next==null){
                   break;
               }
               temp=temp.next;
           }
           //让最后一个节点的next指向添加节点
           //让添加节点的pre指向最后一个节点
           temp.next=heroNode2;
           heroNode2.pre=temp;
   
       }
   ```

   

3. **修改** 

   (1)先找到要修改的节点

   (2)替换

   ![image-20231220212721826](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336744.png)

   ```java
     //修改一个节点的内容
       public void update(HeroNode2 newHeroNode){
           if(head.next==null){
               System.out.println("链表为空");
               return;
           }
           HeroNode2 temp = head.next;
           boolean flag=false;//用于判断是否找到了该节点
           while (temp!=null){
   //            if(temp==null){
   //                break;
   //            }
               //temp与newHeroNode的值相同，则已经找到，变flag为true
               if (temp.no== newHeroNode.no){
                   flag=true;
                   break;
               }
               temp=temp.next;
           }
           //如果找到，则进行修改
           if(flag){
               temp.name= newHeroNode.name;
               temp.nickname= newHeroNode.nickname;
           }else {
               System.out.printf("没有找到编号为 %d 的节点\n",newHeroNode.no);
           }
       }
   ```

   

4. **删除**

(1) 因为是双向链表，因此，我们可以实现自我删除某个节点

(2) 直接找到要删除的这个节点，比如temp

(3) 	temp.pre.next = temp.next

(4) temp.next.pre = temp.pre;

![image-20231220212814930](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336703.png)

```java
    //删除一个节点
    public void del(int no){
        //判断非空
        if(head.next==null){
            System.out.println("这个是个空链表");
            return;
        }
        HeroNode2 temp = head.next;
        boolean flag=false;
        while (temp!=null){
//            if(temp==null){
//                break;
//            }
            //证明已经找到要删除的节点
            if (temp.no==no){
                flag=true;
                break;
            }
            temp=temp.next;
        }
        if(flag){
            //让temp的前一个节点的后指针指向temp的后一个节点
            temp.pre.next=temp.next;
            //首先判断temp是否为最后一个节点
            //再让temp的后一个节点的前指针指向temp的前一个节点
            if(temp.next!=null){
                temp.next.pre=temp.pre;
            }
        }else {
            System.out.println("要删除的节点不存在");
        }
    }
```

总的实现代码

```java
package com.lanqiao.linkedlist;

public class DoubleLinkedListDemo {
    public static void main(String[] args) {
        //测试
        System.out.println("双向链表的测试");
        final HeroNode2 hero1 = new HeroNode2(1, "宋江", "狗贼1");
        final HeroNode2 hero2 = new HeroNode2(2, "李逵", "黑旋风");
        final HeroNode2 hero3 = new HeroNode2(3, "吴用", "智多星");
        final HeroNode2 hero4 = new HeroNode2(4, "林冲", "豹子头");

        final DoubleLinkedList doubleLinkedList = new DoubleLinkedList();
        doubleLinkedList.add(hero1);
        doubleLinkedList.add(hero2);
        doubleLinkedList.add(hero3);
        doubleLinkedList.add(hero4);

        doubleLinkedList.list();

        //修改
        final HeroNode2 hero5 = new HeroNode2(4, "公孙胜", "入云龙");
        doubleLinkedList.update(hero5);
        System.out.println("以下是修改之后的值");
        doubleLinkedList.list();

        //删除
        doubleLinkedList.del(1);
        System.out.println("以下是删除之后的结果");
        doubleLinkedList.list();

    }
}

class DoubleLinkedList {

    //初始化一个节点
    private HeroNode2 head = new HeroNode2(0, "", "");

    //获取头结点
    public HeroNode2 getHead() {
        return head;
    }

    //遍历双向链表
    public void list() {
        // 判断链表是否为空
        if (head.next == null) {
            System.out.println("链表为空");
            return;
        }
        // 因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode2 temp = head.next;
        while (true) {
        // 判断是否到链表最后
            if (temp == null) {
                break;
            }
            // 输出节点的信息
            System.out.println(temp);
            // 将temp后移， 一定小心
            temp = temp.next;
        }
    }

    //在双向链表的尾部添加新的节点
    public void add(HeroNode2 heroNode2){
        HeroNode2 temp = this.head;
        //找到双向链表的尾部
        while (true){
            if(temp.next==null){
                break;
            }
            temp=temp.next;
        }
        //让最后一个节点的next指向添加节点
        //让添加节点的pre指向最后一个节点
        temp.next=heroNode2;
        heroNode2.pre=temp;

    }

    //修改一个节点的内容
    public void update(HeroNode2 newHeroNode){
        if(head.next==null){
            System.out.println("链表为空");
            return;
        }
        HeroNode2 temp = head.next;
        boolean flag=false;//用于判断是否找到了该节点
        while (temp!=null){
//            if(temp==null){
//                break;
//            }
            //temp与newHeroNode的值相同，则已经找到，变flag为true
            if (temp.no== newHeroNode.no){
                flag=true;
                break;
            }
            temp=temp.next;
        }
        //如果找到，则进行修改
        if(flag){
            temp.name= newHeroNode.name;
            temp.nickname= newHeroNode.nickname;
        }else {
            System.out.printf("没有找到编号为 %d 的节点\n",newHeroNode.no);
        }
    }

    //删除一个节点
    public void del(int no){
        //判断非空
        if(head.next==null){
            System.out.println("这个是个空链表");
            return;
        }
        HeroNode2 temp = head.next;
        boolean flag=false;
        while (temp!=null){
//            if(temp==null){
//                break;
//            }
            //证明已经找到要删除的节点
            if (temp.no==no){
                flag=true;
                break;
            }
            temp=temp.next;
        }
        if(flag){
            //让temp的前一个节点的后指针指向temp的后一个节点
            temp.pre.next=temp.next;
            //首先判断temp是否为最后一个节点
            //再让temp的后一个节点的前指针指向temp的前一个节点
            if(temp.next!=null){
                temp.next.pre=temp.pre;
            }
        }else {
            System.out.println("要删除的节点不存在");
        }
    }
}

class HeroNode2 {
    public int no;
    public String name;
    public String nickname;
    public HeroNode2 next; // 指向下一个节点, 默认为null
    public HeroNode2 pre; // 指向前一个节点, 默认为null

    // 构造器
    public HeroNode2(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    // 为了显示方法，我们重新toString
    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + ", nickname=" + nickname + "]";
    }
}
```

### 2、约瑟夫问题

![image-20231222214237595](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336732.png)

问题为：设编号为1，2，… n的n个人围坐一圈，约定编号为k（1<=k<=n）的人从1开始报数，数到m 的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。

**提示**

用一个不带头结点的循环链表来处理Josephu 问题：先构成一个有n个结点的单循环链表，然后由k结点起从1开始计数，计到m时，对应结点从链表中删除，然后再从被删除结点的下一个结点又从1开始计数，直到最后一个结点从链表中删除算法结束。

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336174.png)

![img](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336192.png)

具体代码实现

```c
package com.lanqiao.linkedlist;

public class Josepfu {
    public static void main(String[] args) {
        final CircleSingleLinkedList circleSingleLinkedList = new CircleSingleLinkedList();
        circleSingleLinkedList.addBoy(5);
        //circleSingleLinkedList.showBoy();

        circleSingleLinkedList.countBoy(1,2,5);

    }
}

class CircleSingleLinkedList {
    private Boy first = null;

    /**
     *
     * @param nums 要插入的个数
     */
    public void addBoy(int nums) {
        if (nums < 1) {
            System.out.println("nums的值不正确");
            return;
        }
        //创建一个临时指针,指向链表的最后一个参数
        Boy curBoy = null;
        for (int i = 1; i <= nums; i++) {
            Boy boy = new Boy(i, null);
            //如果链表为空，则直接插入
            if (i == 1) {
                first = boy;
                first.setNext(first);
                curBoy = first;
            } else {//如果链表不为空，则在链表的最后插入新的节点
                curBoy.setNext(boy);
                boy.setNext(first);
                curBoy = boy;
            }
        }
    }

    public void showBoy() {
        //判断链表里是否有数据
        if (first == null) {
            System.out.println("没有数据");
            return;
        }
        //定义一个临时指针指向当前访问的值
        Boy curBoy = first;
        while (true) {
            System.out.printf("小孩的编号为%d\n", curBoy.getNo());
            if (curBoy.getNext() == null) {
                break;
            }
            curBoy = curBoy.getNext();
        }
    }

    public void countBoy(int startNo, int countNum, int nums) {
        if (first == null || startNo < 1 || startNo > nums) {
            System.out.println("参数输入有误");
            return;
        }
        //定义一个临时指针指向first
        Boy helper = first;

        //让helper指针指向链表里的最后一个节点
        while (helper.getNext() != first) {
            helper = helper.getNext();
        }

        //先把两个指针移动到第一个出圈的变量上
        for (int i = 0; i < startNo - 1; i++) {
            first = first.getNext();
            helper = helper.getNext();
        }


        while (true) {
            if (helper == first) {//说明链表中只有一个节点
                break;
            }
            for (int i = 0; i < countNum - 1; i++) {
                first = first.getNext();
                helper = helper.getNext();
            }
            //此时first指向的节点就是要出圈的节点
            System.out.printf("小孩%d出圈\n", first.getNo());
            //出圈的操作
            first = first.getNext();
            helper.setNext(first);
        }
        System.out.printf("此时留在圈的小孩编号为%d\n", first.getNo());
    }
}


class Boy {
    private int no;
    private Boy next;

    public Boy() {
    }

    public Boy(int no, Boy next) {
        this.no = no;
        this.next = next;
    }

    /**
     * 获取
     *
     * @return no
     */
    public int getNo() {
        return no;
    }

    /**
     * 设置
     *
     * @param no
     */
    public void setNo(int no) {
        this.no = no;
    }

    /**
     * 获取
     *
     * @return next
     */
    public Boy getNext() {
        return next;
    }

    /**
     * 设置
     *
     * @param next
     */
    public void setNext(Boy next) {
        this.next = next;
    }

    public String toString() {
        return "Boy{no = " + no + ", next = " + next + "}";
    }
}
```


