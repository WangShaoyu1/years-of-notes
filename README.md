Promise����д��ES6�Ĺ淶���У��ṩ��������һ�ָ����ѺõĶ����첽��̵Ľ������������֮ǰ���ʹ�õ��ǻص��������¼���ʵ���첽��̡�

        ��ô�����Promise�����أ��������ES6�¼����С��飬��������һ������������Array��Date��Math��Global�ȶ���һ���Ķ�����ֻ����Promise������Ϊ�첽��̵�һ�ֽ�������������ǲ�����õ��첽��̽������������Ժ������ۡ�
        ����Ϊ��Ϊ�˽���첽��̣���Ӧ�ģ�Promise��������״̬�ģ��¶��ǰ���˻��ǰ첻�ã���Ҫ�и�״̬����ʶ��Promise�����״̬��ͨ����ִ��new Promse()��ʱ�����������������������ִ���������ģ����£�
       
const promise=new Promise((resolve,reject)=>{
    if(.....)
       resolve(argv1)
    else
       reject(argv2)
})


�������ӵ��У�resolve��reject��ʾ����promise��������е�����״̬���ֱ��ʾ���ɹ��ˡ��͡�ʧ���ˡ�����Ȼ����һ��״̬�ǡ������С��������Promise������û�취���Ա�ʾ�������ܽ���ǣ�
Promise����������״̬��resolve������ˣ������ˣ���reject���Բ��𣬱����ˣ���pending���������𣬱��ż���������״̬��
������״̬��ô�����أ�
�Ϳ��������뵽new Promise�����еĲ���������һ��������ʾ״̬�Ѿ���ɣ��ڶ�����ʾ״̬�Ѿ�ʧ�ܣ�����һ��������ִ���ˣ�resolve������ִ�У����ˣ����ɵ�Promise�����״̬�����Ѿ���ɣ�reject������ִ�У�ŷ�ˣ����ɵ�Promise�����״̬����ʧ���ˣ������״̬���� �����С�
ע�⣺1.Promise�����״̬һ�������ˣ��Ͳ��ܸı���
           2.Promise�����״̬ת��ֻ�������ַ�ʽ��pending����>resolve��������---->����ˣ���pending����>reject��������----->ʧ���ˣ�����������������
˵�õ��첽����أ���ȥ�ˣ��𼱣����ľ��Ǿ����첽�������ˡ�
������һ��Promise����ֻ��һ��ǰ�ࡣPromise����prototype��������������then������catch������then����������ʾ���ǡ����������ˣ�Ȼ���ɶ�ء�����һ�����첽����then������������������������������Promise�������ƣ�
then������catch���������еĲ�������������Promsie����ʱ��ִ��resolve��reject����ʱ����Ĳ�����
�������£�

then((resolve)=>{
    //do something
    //Promise����״̬Ϊ[b]�����[/b]��ʱ��ִ��
},(reject)=>{
     //do something
    //Promise����״̬Ϊ[b]ʧ����[/b]��ʱ��ִ��
})

���ǲ��Ƽ���������д�����Ƽ�����д����

then((resolve)=>{
    //do something
    //Promise����״̬Ϊ[b]�����[/b]��ʱ��ִ��
}).catch((reject)=>{
    //do something
    //Promise����״̬Ϊ[b]ʧ����[/b]��ʱ��ִ��
})

�̵�����ô�࣬�������⣺
��Ŀ1��

console.log(1);
new Promise(function (resolve, reject){
    reject(true);
    window.setTimeout(function (){
        resolve(false);
    }, 0);
}).then(function(){
    console.log(2);
}, function(){
    console.log(3);
});
console.log(4);

����1 4 3
�����漰����֪ʶ���У�
1.new Promise �����ʱ�򣬲����������ڲ�����ִ���˵ڶ�����������һ����������������ɵ�Promise�����״̬Ϊ��ʧ���ˣ����ޱ������״̬�Ŀ����ԣ�
2.��Promise�������ɺ�ִ�е�then����ʱ������Promise�����״̬Ϊ��ʧ���ˡ�������ִ��then�еĵڶ�������������
3.then�������첽����������������ʱ��Ҫ�ȵȵ�����ͬ������ִ������Ժ���ִ�У����ԣ���˳����ִ��console.log(1)��console.log(4)��Ȼ����ִ��console.log(3)

��Ŀ2��

new Promise((resolve,reject)=>{
    var bool=true;
    if(bool){
        resolve("yes,good")
    }else{
        reject("no,bad")
    }
}).then((msg)=>{
    console.log("msg:",msg);
    console.log("unknown:",unknown)
}).catch((err)=>{
    console.log("error:",err)
}).then((msg1)=>{
    console.log("msg1:",msg1)
}).then(()=>{
   return "�Ҳ���undefined"
).then((msg)=>{
    console.log("msg:",msg);
).then(()=>{
    console.log("��������")
})


���ǣ�msg: yes,good
             error: ReferenceError: unknown is not defined(��)
             msg1: undefined
             msg: �Ҳ���undefined
             ��������
�����漰����֪ʶ���У�
1.then�����е������ص������еĲ���������Promise�����ʱ����ģ����Ե�һ��then�����е�msg��ʵ����"yes,good"��
2.catch�Ჶ׽��֮ǰ���ֵ����д���Ҳ����˵ǰ���promise�г��ֵĴ����һֱð�ݵ����棬ֱ����catch�����գ�
3.then,catch����ִ�к�᷵��һ���������Promsie������Ҳ����ʽд���Ļ������������������Promise���󵽵����ĸ���������ô��⣺
    a.���thenû�з���ֵ����ô���ص�����һ���յ�Promise������Promise.resolve()����Promise.reject()ֱ������
    b.���then�з���ֵdata����ô���صľ���Promise.resolve(data)��Promise�������Ч��new Promise((resolve){resolve(data)}),reject��������ƣ���ͽ����˵ڶ��δ�ӡmsg��ʱ
       ������ֵ�ģ�������������then���ص�Promise�����resolve���� �Ĳ��������ݸ������then����ʹ��


��Ŀ3��
var p1 = new Promise((resolve, reject) => {
    setTimeout(function () {
        reject("������")
    }, 1000)
});
var p2 = new Promise((resolve, reject) => {
    resolve(p1)
});
p2.then((msg) => {
    console.log("msg:", msg)
}).catch(() => {
    console.log("������")
})



���ǣ�������
�����漰����֪ʶ���У�
1.��resolve�еĲ�����һ��Promise�����ʱ��Ҫ�ȵ���Promise�����״̬ȷ��������˻���ʧ���ˣ����ܽ�����һ����������״̬��������һ��Promise�����״̬��
  �����У�p2��״̬Ҫ�ȵ�p1��״̬ȷ����ž�����Ȼ��p1��״̬�� ʧ���ˣ�p2��״̬Ҳ��ʧ�ܵģ����Ҫע�⡣

����Promise���󣬻�������API�����о�������Promise.all��Promise.race�ȵȡ�
�����첽��̣�Generator�����Լ�ES7��async�������Ǹ��õ�ʵ�ַ�����

д�����Ļ�
Javascript��Խ��Խ��������ԣ���Ϲ���������������ԭ��֧����Խ��Խ������ԣ�Խ��Խǿ�󣬶�����ҵ��Ӧ�ÿ������ӵ���Ӧ�֡�
��ǰ�ˣ����ּ򵥣������׶���Ҫ��֪ʶ�ṹ����˵��Java,c++ �����Ը��࣬���ӣ�
ֻ�ǻ���ҳ�棬����ǰ����˵�Ǻܻ����Ĺ�������Ϲ�����Ҫ�������Ҫ��������������̻����Ż����ܣ����ֺ�̨���ܣ�
�Ѿ��в��������ȶ����Զ�����˷���ƽ̨������Ҫ���ͯЬ����ϣ����ģ����԰���������ã�������ֱ�����ȶ����Ʒ��񣬸���Ĺ���������ǰ�ˣ����飬�û����飬�ɵ���˲���˵����˵Ĺ���������������
ǰ�˽��������ֲ������������������ӿ�����ٵ���������Ч��Խ��Խ�ߣ����ǲ����е���ս��ʱ�ڰټ����������ʱ�ڵİٻ���š�
����ǰ����õ�ʱ��������������Javascriptһͳ������ʱ��������˵�������Բ������ˣ�����˵Javascriptһ֧���󣩣�
����Ԥ�⣬������ٿ���λ���
���㡣

 

