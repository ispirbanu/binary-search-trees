# binary-search-trees
ikili arama ağaçları C kodlarıyla ekleme silme ve dolaşma işlemleri

- **********************************İkili arama ağaçları binary searh tree**********************************
    
    ### ekleme ve ağaçta dolaşma
    
    ```c
    // Online C compiler to run C program online
    #include <stdio.h>
    #include <stdlib.h>
    
    struct n{
        int data;
        struct n* sol;
        struct n* sag;
    };
    
    struct n * ekle(struct n* agac, int x){
        //ağaç boş ise
        if(agac==NULL){
            struct n* root =(struct n*) malloc (sizeof(struct n*));
            root->sol= NULL;
            root->sag=NULL;
            root->data=x;
            return root;
        }
        //eklenecek değer düğümden büyükse sağına gidilir.
        if(agac->data <x){
            //burada aslında recurif yapı şu şekilde kullanılır.
            //veri düğümden büyükse sağına eklenecek böylece sağ taraf tekrar bir ağaç gibi düşünülür ve tüm düğümler bu şekilde eleman kalmama durumuna kadar devam eder.
            agac->sag=ekle(agac->sag,x);
            return agac;
        }
        //aynı şey küçük olma durumunda sol için geçerli.
        agac->sol=ekle(agac->sol,x);
        return agac;
        
    }
    void dolas(struct n* agac){
        if(agac==NULL)
            return;
        dolas(agac->sol);
        printf("%d \n", agac->data);
        dolas(agac->sag);
        // printf("%d",agac->data);
    }
    
    int main() {
        struct node*agac=NULL;
        agac=ekle(agac,12);
        agac=ekle(agac,200);
        agac=ekle(agac,190);
        agac=ekle(agac,213); 
        agac=ekle(agac,56);
        agac=ekle(agac,24);
        dolas(agac);
        
        return 0;
    }
    //sonuc
    //12 
    // 24 
    // 56 
    // 190 
    // 200 
    // 213
    ```
    
    burada dolaşma prefix olarak (LNR) şeklinde dolaşıldı.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ce368ba4-e6a6-4112-af31-26b55c5ca606/f53c0072-474a-42a0-b5e4-a802475788f1/Untitled.png)
    
    Yukarıdaki görsel örnek olursa;
    
    Ağaçta dolaşmanın kısa tanımı olarak 
    
    - **infixte; LNR,RNL (nodun ortada olduğu) üstteki dolaşma şekli LNR olaraktı**
    - ****************************************************************************Prefixte; NLR,NRL (nodun başta olduğu)****************************************************************************
    - **postfixte; LRN,RLN (nodun sonda olduğu)**
    
     
    
    ### Arama işlemi
    
    Arama işlemi dolaşma işlemine benzer 
    
    burada bulunma durumunda 1 bulunmama durumunda -1 döndürerek arama işlemi yapıldı.
    
    ```c
    //ağaçta değer arama
    int bul (struct n* agac, int aranan){
         if(agac==NULL)
            return -1;
        if(agac->data==aranan){
            return +1;
        }
        if(bul(agac->sag,aranan)>0)
            return 1;
        if(bul(agac->sol,aranan)>0)
            return 1;
        return -1;
    }
    **int main() {
        struct node*agac=NULL;
        agac=ekle(agac,12);
        agac=ekle(agac,200);
        agac=ekle(agac,190);
        agac=ekle(agac,213); 
        agac=ekle(agac,56);
        agac=ekle(agac,24);
        dolas(agac);
        
        printf("arama sonucu: %d",bul(agac,100));
        printf("arama sonucu: %d",bul(agac,24));
        return 0;
    }
    //sonuç
    //arama sonucu: -1
    //arama sonucu: 1**
    ```
    
    ### min ve max bulmak
    
    ağacın en solunda düğümden küçük değerler sağında ise büyük değerler bulunduğundan max aranırken ağacın sağ düğümlerinde null olana kadar
    
    min aranırken sol düğümlerinde null olana kadar dolaşılır.
    
    ```c
    int max(struct n* agac){
        while(agac->sag!=NULL){
            agac=agac->sag;
        }
        return agac->data;
    }
    int min(struct n* agac){
        while(agac->sol!=NULL){
            agac=agac->sol;
        }
        return agac->data;
    }
    int main() {
        struct node*agac=NULL;
        agac=ekle(agac,12);
        agac=ekle(agac,200);
        agac=ekle(agac,190);
        agac=ekle(agac,213); 
        agac=ekle(agac,56);
        agac=ekle(agac,24);
        dolas(agac);
       
        printf("max: %d \n",max(agac));
        printf("min: %d \n",min(agac));
        
        return 0;
    }
    
    //sonuc 
    //max: 213 
    //min: 12
    ```
    
    ### Silme işlemi
    
    ```c
    struct n* sil(struct n* agac, int x){
        if(agac==NULL)
            return NULL;
        if(agac->data==x){
            if(agac->sol==NULL && agac->sag==NULL)
                return NULL;
            //sağındave solunda değer varsa ya soldaki en büyük ya da sağdaki en küçük değer silinecek düğümün yerine alınır.
            if(agac->sag !=NULL){
                agac->data=min(agac->sag);
                agac->sag=sil(agac->sag,min(agac->sag));
                return agac;
            }
          //diğer olasılıksa sağında değer olmaması durumudur bu sefer de soldaki max değer düğüme atanır ve o değer silinir.
            agac->data=max(agac->sol);
            agac->sol=sil(agac->sol,max(agac->sol));
            return agac;
            }
            //gelen silinecek değer düğümden büyükse
            if(agac->data <x){
                agac->sag=sil(agac->sag,x);
                return agac;
            }
            //gelen silinecek değer düğümden küçükse
            agac->sol=sil(agac->sol,x);
            return agac;
            
            
        }
    
    int main() {
        struct node*agac=NULL;
        agac=ekle(agac,12);
        agac=ekle(agac,200);
        agac=ekle(agac,190);
        agac=ekle(agac,213); 
        agac=ekle(agac,56);
        agac=ekle(agac,24);
        dolas(agac);
        
        printf("arama sonucu: %d \n",bul(agac,100));
        printf("arama sonucu: %d \n",bul(agac,24));
        printf("max: %d \n",max(agac));
        printf("min: %d \n",min(agac));
        
        agac=sil(agac,190);
        dolas(agac);
        // sonuç;
        // 12 
        // 24 
        // 56 
        // 200 
        // 213 
        return 0;
    }
    ```
