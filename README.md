# htrt

import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class firtestore extends StatefulWidget {


  @override
  State<firtestore> createState() => _firtestoreState();
}

class _firtestoreState extends State<firtestore> {
  final user = FirebaseAuth.instance.currentUser;
  List<String>docsIDS=[];

  Future getdocsIDS  ()async{
    await FirebaseFirestore.instance.collection('users').get().then((snapsot) => snapsot.docs.forEach((snapsot) {
      print(snapsot.reference.id);
      docsIDS.add(snapsot.reference.id);
    }));
  }
  @override
  Widget build(BuildContext context) {


    return
      Scaffold(

      appBar: AppBar(
        title:const Text('firstore'),
        actions: [

        ],
      ),
      body:
       FutureBuilder(
              future: getdocsIDS(),
              builder: (context, snapshot) {
                return ListView.builder(
                  itemCount:docsIDS.length,
                    itemBuilder: (context,index){
                  return ListTile(
                    title: GetTaskDetail(documentId: docsIDS[index],)
                  );
                }
                );
              }),
      );
  }
}

class GetTaskDetail extends StatelessWidget {
  final String documentId;
  const GetTaskDetail({ required this.documentId});

  @override
  Widget build(BuildContext context) {
    CollectionReference users = FirebaseFirestore.instance.collection('users') ;
    return FutureBuilder(
      future: users.doc(documentId).get(),
        builder:
        (context,snapshot){
      if (snapshot.connectionState==ConnectionState.done) {
        Map<String, dynamic> data =snapshot.data!.data() as  Map<String, dynamic>;
        // return Text('your name${data['name']}');
        return Column(
          children: [
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Container(
                width: MediaQuery.of(context).size.width,
                height: 100,
                child: Center(child: Text('your name :${data['name']}'),),
              ),
            ),
          ],
        );
      }
      return Center(child: CircularProgressIndicator());
    }

    ); }
  }


