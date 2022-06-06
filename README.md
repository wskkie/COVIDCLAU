from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

@app.route('/',methods=['GET','POST'])
def home():

    ask = [
       "Apakah Suhu badan anda > 36?",
       "Apakah anda mengalami Batuk?",
       "Apakah Badan anda Panas?",
       "Apakah anda merasa Sakit Badan / Pegal / Linu?",
       "Apakah anda merasa Sakit Kepala?",
       "Apakah anda mengalami Sakit Tenggorokan?",
       "Apakah anda mengalami Kehilangan Penciuman?",
       "Apakah anda merasakan Sesak Nafas?",
       "Apakah anda mengalami Mual / Muntah?",
       "Apakah anda Pilek?",
       "Apakah anda Meriang?",
    ]

    leng_ask = len(ask)

    rules = [
        [1,1,1,1,1,1,1,0,0,0,0],
        [1,0,0,1,0,0,1,0,0,0,0],
        [1,0,1,0,1,1,0,1,0,0,0],
        [0,1,0,1,1,0,1,1,0,0,0],
        [1,1,1,1,1,0,0,0,0,0,0],
        [1,0,1,0,1,0,0,0,1,0,0],
        [1,1,1,1,0,0,0,0,0,1,0],
        [1,1,0,1,0,0,0,0,0,1,1],
        [1,1,1,0,0,1,0,0,0,0,0],
        [1,1,1,1,1,1,1,1,0,0,1],
        [1,1,1,1,1,1,0,1,1,0,1],
        [0,1,0,1,1,1,1,1,1,0,1],
        [0,1,0,1,0,1,1,1,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0], 
        [1,1,1,1,1,1,1,1,1,1,1],
        [0,1,0,1,1,1,1,1,0,0,1], 
        [1,1,1,0,1,0,1,1,0,0,0],
        [0,0,0,0,0,0,0,0,0,0,0],
        [1,1,1,1,1,0,1,1,1,0,0],
        [1,0,0,0,1,0,0,0,0,0,1],
        [1,1,1,1,1,1,1,1,1,1,1],
        [0,1,0,1,0,0,0,0,0,0,0],
    ]


    if request.method == 'POST':
        req = dict(request.form)
        req_key = list(req.keys())
        req_val = list(req.values())
        # convert_key_int = list(map(int, req_key))
        convert_val_int = list(map(int, req_val))
        # data = dict(zip(convert_key_int, convert_val_int))

        penyakit = ["P02","P02","P03","P01","P02","P02","P02","P01","P02","P03","P03","P02","P01","P01","P03","P02","P02","P02","P01","P03","P01",]


        jawaban = []
        n = 0
        for i in range(len(rules)):
            n = 0
            for j in range(len(rules[i])):
                if convert_val_int[j] == rules[i][j]:
                    n += 1
            jawaban.append(n)
        
        rata2 = list(map(lambda x : x / 21 * 100, jawaban))

        result = list(zip(penyakit, rata2))
        print(convert_val_int)
        print("===========")
        for k in result:
            print(k)
        print("===========")
        get_max_data = list(max(result, key=lambda item:item[1]))
        
        
        diagnosa = {
            "P01" : "Ringan",
            "P02" : "Sedang",
            "P03" : "Berat",
        }
        hasil_diagnosa = get_max_data[0]
        hasil = diagnosa.get(hasil_diagnosa)
        print(hasil)
        return render_template('utama.html', ask=ask, leng_ask=leng_ask, hasil=hasil)
    else:
        return render_template('utama.html', ask=ask, leng_ask=leng_ask)

if __name__ == '__main__':
    app.run(host = 'localhost',debug=True)
