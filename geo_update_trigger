CREATE TABLE bdtkukar (
	id character varying(100),
	bdt_id character varying(100),
	sumber_bdt character varying(100),	
	provinsi_id character varying(100),
	kabupaten_id character varying(100),
	kecamatan_id character varying(100),
	desa_id character varying(25),
	kecamatan_name character varying(100),
	desa_name character varying(100),
	alamat character varying(255),
	nama_kepala_rumah_tangga character varying(100),
	nik_ktp character varying(100),
	pekerjaan character varying(100),
	jumlah_art character varying(100),
	jumlah_keluarga character varying(100),
	status_bangunan character varying(100),
	status_lahan character varying(100),
	luas_lantai character varying(100),
	luas_lantai_m2 character varying(100),
	jenis_lantai_terluas character varying(100),
	jenis_dinding character varying(100),
	kondisi_dinding character varying(100),
	jenis_atap character varying(100),
	kondisi_atap character varying(100),
	jumlah_kamar character varying(100),
	sumber_air_minum character varying(100),
	cara_peroleh_air_minum character varying(100),
	sumber_penerangan_utama character varying(100),
	daya_listrik character varying(100),
	norek_pln character varying(100),
	fasilitas_bab character varying(100),
	kloset character varying(100),
	tempat_pembuangan_akhir_tinja character varying(100),
	rumah_ditempat_lain character varying(100),
	status_verifikasi character varying(225),
	verifikasi character varying(225),
	catatan_verifikasi character varying(225),
	attachment_data character varying(225),
	penyasar character varying(255),
	status_sasaran character varying(225),
	tahun_kegiatan character varying(225),
	gbr_sebelum character varying(225),
	gbr_sesudah character varying(225),
	latitude double precision,
	longitude double precision,
	geom geometry,
	geog geography,
	updated_ts double precision,
	CONSTRAINT bdtkukar_unique_key PRIMARY KEY (id)
) ;

CREATE INDEX bdtkukar_geom_idx
  ON bdtkukar
  USING gist
  (geom);

CREATE INDEX bdtkukar_geog_idx
  ON bdtkukar
  USING gist
  (geog);
  
CREATE OR REPLACE FUNCTION fn_bdtkukar_geo_update_event() RETURNS trigger AS $fn_bdtkukar_geo_update_event$
  BEGIN  
	-- as this is an after trigger, NEW contains all the information we need even for INSERT
	UPDATE bdtkukar SET 
	geom = ST_SetSRID(ST_MakePoint(NEW.longitude,NEW.latitude), 4326),
	geog = ST_SetSRID(ST_MakePoint(NEW.longitude,NEW.latitude), 4326)::geography,
	updated_ts = date_part('epoch'::text, now()) WHERE id=NEW.id;

	RAISE NOTICE 'UPDATING geo data for %, [%,%]' , NEW.id, NEW.latitude, NEW.longitude;	
    RETURN NULL; -- result is ignored since this is an AFTER trigger
  END;
$fn_bdtkukar_geo_update_event$ LANGUAGE plpgsql;

CREATE TRIGGER tr_bdtkukar_inserted
  AFTER INSERT ON bdtkukar
  FOR EACH ROW
  EXECUTE PROCEDURE fn_bdtkukar_geo_update_event();
  
CREATE TRIGGER tr_bdtkukar_geo_updated
  AFTER UPDATE OF 
  latitude,
  longitude
  ON bdtkukar
  FOR EACH ROW
  EXECUTE PROCEDURE fn_bdtkukar_geo_update_event();
  
  select * from PUBLIC.bdtkukar;
COPY PUBLIC.bdtkukar FROM '/home/.../bdtkukar_bsps.csv' WITH CSV HEADER;
