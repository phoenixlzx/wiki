title: OpenNebula
category: Cloud Computing
---

#### Enable KVM CPU Passthrough

Attributes need to be added:

```
RAW=[ DATA="<cpu mode='host-passthrough'/>", TYPE="kvm" ]
```

1. Choose your VM template
2. Click on 'UPDATE'
3. Click on 'Other'
4. From the 'RAW data' dropdown menu, choose 'KVM'
5. In the DATA text box, insert:

```
"<cpu mode='host-passthrough'/>"
```

References:

* [OpenNebula Backlog](http://dev.opennebula.org/issues/2869)

