using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using System.Web.Script.Serialization;
//using System.Windows;
//using System.Windows.Threading;

namespace MessagingKit {
    public class MKMessageServer {

        private IPAddress ip;
        private int port;

        private bool stopflag = false;
        private bool running = false;

        //private NotificationManager notifyManager;
        private IMKMessageServerDelegate messageServerDelegate;
        private Thread listeningSocketThread;
        private byte[] buffer = new byte[256];
        //private JavaScriptSerializer serializer = new JavaScriptSerializer();
        //public static List<string> messages = new List<string>();

        public bool Running {
            get { return this.running; }
        }
        /*
        public int MessageCount {
            get { return notifyManager.NotificationCount; }
        }*/

        public string IP {
            get { return this.ip.ToString(); }
            set { this.ip = IPAddress.Parse(value); }
        }

        public int Port { get; set; }
        public IMKMessageServerDelegate MessageServerDelegate {
            get { return this.messageServerDelegate; }
            set { this.messageServerDelegate = value; }
        }

        public MKMessageServer(string ip, int port) {
            //System.Environment.
            //ip = IPAddress.Parse("127.0.0.1");
            this.ip = IPAddress.Parse(ip);
            this.port = port;
        }
        
        public bool Start(){
            //if(listeningSocketThread != null && listeningSocketThread.IsAlive) {
            if(this.running) {
                //MessageBox.Show("Thread already running");
                return false;
            }
            
            stopflag = false;
            listeningSocketThread = new Thread(delegate(){
                this.listen();
            });

            //notifyManager.Start();
            listeningSocketThread.Start();
            
            if(!listeningSocketThread.IsAlive) {
                //vm.State = "Start Failed";
                this.messageServerDelegate.messageServerDidFailedToStart(this, MKMessageServerStatus.StartFailed);
                return false;
            }
            this.running = true;
            //vm.State = "Running";
            //vm.StateColor = "#00B33C";
            this.messageServerDelegate.messageServerDidStart(this, MKMessageServerStatus.Running);
            
            return true;
        }

        public bool Stop(){
            stopflag = true;
            //notifyManager.Stop();
            /*
            for(int i = 0;i <= 10;i++) {
                MessageBox.Show("MessageServer Mainthread IsAlive:" + listeningSocketThread.IsAlive.ToString());
            }*/

            //listeningSocketThread.Abort();
            //if(listeningSocketThread.IsAlive && notifyManager.Stopped) {
            if(listeningSocketThread.IsAlive) {
                //vm.State = "Stopped";
                //vm.StateColor="#FFD11A";
                this.messageServerDelegate.messageServerDidStop(this, MKMessageServerStatus.Stopped);
                return false;
            }
            return true;
        }

        private int listen(){
            TcpListener server = new TcpListener(this.ip, this.port);
            server.Start();
            //while(true) 
            while(!stopflag){
                if(!server.Pending()) {
                    Thread.Sleep(5000);
                    continue;
                }
                
                TcpClient client = server.AcceptTcpClient();
                //server.BeginAcceptTcpClient
                
                if(stopflag){
                    break;
                }

                Thread socketThread = new Thread(() => {
                    NetworkStream stream = client.GetStream();
                    stream.ReadTimeout = 10;
                    byte[] buffer = new Byte[1024];
                    string data = "";
                    
                    while(stream.DataAvailable) {
                        stream.Read(buffer, 0, buffer.Length);
                        data = data + Encoding.ASCII.GetString(buffer);
                    }

                    client.Close();

                    //string jsonData=Encoding.ASCII.GetString(buffer);
                    //MessageBox.Show(jsonData);
                    data = data.TrimEnd('\0');

                    if(data.Length <= 2) {
                        return;
                    }
                    /*
                    if(data.Substring(0,1) != "{" || data.Substring(data.Length - 1) != "}") {
                        return;
                    }
                    */

                    //JavaScriptSerializer serializer = new JavaScriptSerializer();
                    //Dictionary<string, string> pkt = serializer.Deserialize<Dictionary<string, string>>(data);
                    ////this.notifyManager.AddNotification(pkt);
                    
                    /*
                    Application.Current.Dispatcher.BeginInvoke(new Action(() =>{
                        //NotificationWindow notification = new NotificationWindow(pkt);
                        //MessageBox.Show("New Message\nmessageServer.notifyManager.ClosedFlag = " + this.notifyManager.ClosedFlag.ToString());
                        //NotificationWindow notification = new NotificationWindow(pkt, this.NotificationManager.ClosedFlag);
                        NotificationWindow notification = new NotificationWindow(pkt, this.notifyManager);
                        //this.notifyManager.ClosedFlag
                        this.notifyManager.AddNotification(notification);
                        //test.Show();
                    }));
                    */

                    this.messageServerDelegate.messageServerDidReceive(this, data);

                    //messages.Add(data);
                });

                socketThread.Start();
            }

            server.Stop();
            
            //vm.StateColor = "#FFF22F2F";
            this.running = false;
            this.messageServerDelegate.messageServerDidStop(this, MKMessageServerStatus.Stopped);

            //return messages.Count;
            return 0;
        }

    }
}
