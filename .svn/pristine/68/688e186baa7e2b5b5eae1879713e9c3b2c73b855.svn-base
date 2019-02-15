using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace MessagingKit {
    public interface IMKMessageServerDelegate {
        void messageServerDidStart(MKMessageServer server, MKMessageServerStatus status);
        void messageServerDidStop(MKMessageServer server, MKMessageServerStatus status);
        void messageServerDidFailedToStart(MKMessageServer server, MKMessageServerStatus status);
        void messageServerDidReceive(MKMessageServer server, string message);
    }
}
