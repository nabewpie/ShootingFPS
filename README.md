# ShootingFPS


using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GunPickup : MonoBehaviour
{
    public Transform gun;
    public Transform GunController;
    public Transform cam;
    public float range = 100f;
    Rigidbody rigidBody;
    public float PickRange;
    enum EquipState {Bare, HasGun}
    EquipState equipState;
    Collision collision;
    public AudioSource GunShot;
    public AudioSource GunDrop;
    void Start()
    {
    
    }

    void Update()
    {
        if (Input.GetKey(KeyCode.E))
        {
            Equip();
        }
        if (Input.GetMouseButtonDown(0))
        {
            Fire();
        }

        if (Input.GetKey(KeyCode.Q))
        {
            Drop(); 
        }
    }

    private void Fire()
    {
        if (equipState == EquipState.HasGun) {
            RaycastHit hit;
            if (Physics.Raycast(cam.transform.position, cam.transform.forward, out hit, range))
            {
                print("I hit: " + hit.transform.name);
            }
        }
        GunShot.Play();
    }

    

    void Drop()
    {
        gun.transform.parent = null;
        gun.gameObject.AddComponent<Rigidbody>();
        equipState = EquipState.Bare;
        GunDrop.Play();

        
    }

    private void PickupRay()
    {
        RaycastHit PickupHit;
        Physics.Raycast(cam.transform.position, cam.transform.forward, out PickupHit, PickRange);

        
    }
   
    


    void Equip()
    {
        if (gun.gameObject.tag == "pickup")
        {
            PickupRay();
            gun.position = GunController.position;
            gun.transform.parent = GunController;
            gun.rotation = GunController.rotation;
            equipState = EquipState.HasGun;
            Destroy(gun.GetComponent<Rigidbody>());
        }
    }

    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.tag == "ground")
        {
            Destroy(gun.gameObject.GetComponent<Rigidbody>());
        }

        

    }

    


}
